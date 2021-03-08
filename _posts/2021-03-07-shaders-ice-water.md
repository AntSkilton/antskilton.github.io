---
layout: single
title:  "Shaders: Ice & Water"
excerpt: "Experimenting with custom H2O."
categories: shaders
toc: true
---
## Ice Shader

As a translucent surface, ice often looks flat without a simulated depth to it. I wanted to simulate depth when motion is applied to the camera, and for it to run on an unlit shader with a custom lighting function. This is the final result.

![image-center](\assets\images\2021-03-07-shaders-ice-water\iceShader.gif){: .align-center}

To begin, I made a tilable ice diffuse in Substance Designer which has these high contrast lines with a softer variation with contrast of noise sitting behind it. This separation will help the illusion when the samples and offset properties are tweaked.

![image-center](\assets\images\2021-03-07-shaders-ice-water\iceDiffuse.png){: .align-center}

### Depth

We run a [Texture Sampler State function](https://docs.unity3d.com/Packages/com.unity.shadergraph@6.9/manual/Sample-Texture-2D-LOD-Node.html) passing in the diffuse, sampler state and the UV arguments. Next, the UVs are then added to the offset multiplied by the world space camera positions, minus the absolute world position for both X and Z respectively. Subtracting these vectors gives us this starburst depth effect.

We repeat that process for each sample we have. Currently the output would be very blurry. I've found that 24 samples (8 would be more optimal) with a -0.01 offset has the highest fidelity. With a positive offset of 0.01 you get a god ray effect, could be useful for phantasmal fantasy surfaces. We still want the original texture sharpness in there to support the cracks, so the render output is a lerp between the original texture with this blurred sample function with an exponent of 0.63 (favouring the cracks). If we ever wanted sharper lerp blends, I'd have to bring in a mask for the cracks, but I think it reads well enough.

### Lighting

Seeing as we're not using a normal map, this unlit shader is fit for Quest 2 / mobile. It takes the scene's main directional light in with [GetMainLight()](https://github.com/Unity-Technologies/Graphics/blob/master/com.unity.render-pipelines.universal/ShaderLibrary/Lighting.hlsl#L130) so we can take in it's light vector. The smoothness is `exp2(10 * Smoothness Property + 1)`. This creates a nice exponential curve with the +1 normalising it between 0 and 1. We feed in and normalise the vertex world normal and the camera's world normal. For the output we run this [LightingSpecular()](https://github.com/Unity-Technologies/Graphics/blob/master/com.unity.render-pipelines.universal/ShaderLibrary/Lighting.hlsl#L738) function, taking in the arguments of a normalised light direction vector, the vertex world and camera view normals (already normalised), specular colour tint and the smoothness for the highlight size.

![image-center](\assets\images\2021-03-07-shaders-ice-water\icePasses.gif){: .align-center}

## Water Shader

There are many ways to implement water with a high ceiling for intricate solutions. My focus here was to build from a vanilla unlit shader which has readable stylised water qualities properties including flow, intersection edge foam and a slope-based waterfall foam.

![image-center](\assets\images\2021-03-07-shaders-ice-water\stylisedWater.gif){: .align-center}

### Base Water

I began with a simple tilable strip of blue hues I made in Substance Designer. By not working with pre morphed patterns, it allows the shader to distort this pattern over an interactive property in world space without tearing and creating artifacts. We could make a 4x1 texture with the distortion waves built in if we didn't want to calculate a procedural in the shader however.

![image-center](\assets\images\2021-03-07-shaders-ice-water\waterDiffuse.png){: .align-center}

We start with morphing the UVs of the texture by said procedural noise. The UVs of the noise pan over time in the world position's X and Z vectors. This does mean it has a hardcoded direction, but if the artist maps out their UVs in a strip with clean geometry like in the screenshot below then they'll always get a clean flow direction for water regardless of geometry shape. A shader variation would be required if we wanted a still body of water like a lake with subtle omni directional movement.

![image-center](\assets\images\2021-03-07-shaders-ice-water\riverUVFlow.png){: .align-center}

### Slope Foam

Recycling the time multiplied by panning over the X and Z world co-ordinates logic from the base diffuse, we can feed that into a gradient noise for movement. We add this with the world normal vector and feed that into one argument of a dot product. The other argument for the dot product is a straight up Y vector. This means as the vertex normals of the mesh rotate in world space, this effects the visibility of the effect. By selecting the Y vector, we say that the effect will be "off" when the vectors align. This works for us as we only want it to appear on slopes with the largest X and Z weighting without having to worry about mesh rotation in world space. If mesh vertices are perpendicular to Z/X then some extreme stretches would occur, so a slant of 15-30 degrees sits well within the dot product's range and time speed properties.

![image-center](\assets\images\2021-03-07-shaders-ice-water\waterSlope.gif){: .align-center}

To create the mask of the procedural noise, we want to clamp two thresholds together to create a band similar to blending a couple histogram scans to create a edge mask in Substance Designer (that being said, making this mask offline and feeding it in would be cheaper but less interactive). Using a [smoothstep](https://docs.unity3d.com/Packages/com.unity.shadergraph@7.3/manual/Smoothstep-Node.html) function and subtracting -0.1 from each of the two threshold ends, we can multiply them together to create the difference (which is the clamped band range we see). This can be done cheaper by one-minusing one of the smoothsteps (to create the inversion), and using that one threshold to uniformly offset either side for the ripple thickness. I appreciate the drops of white noise on the near perpendicular to Y surfaces as this effect slightly creeps in onto the river and grows into the waterfall foam.

### Foam Edging

Up until now this could be created with an opaque shader, but we'll now be using the camera's Z-depth data, so we'll be using a transparent surface shader type instead of opaque to create this effect. The output is still fed into colour with a full white alpha. This was a _gotcha_ for me with working with translucency in a SRP, remember to enable `Depth Texture` on your render pipeline asset for scene depth to work.

There are two effects added together for this outline to appear complete before being added to the above functions. There's the edge detection fringe function, and then there's a standard fresnel.

![image-center](\assets\images\2021-03-07-shaders-ice-water\outlineEffect.png){: .align-center}

For the edge detection, first subtract the camera's position vector from the world position's vector. Feed that into a dot product with the camera's direction vector. This needs to be normalised, so remap that between the camera's far plane Y vector (depth) as the minimum and 1 (on Y only) as the maximum.

Next we need a parameter to control the offset of this edging (the effect falls apart if it goes too far). To create this we repeat what we did above but include the exposed property and add it between the dot product and the remap so the property scales the offset value proportionately.

We then add these two edges into a smoothstep with the scene depth as the input. All of the effect needs to inverted to create a fresnel like effect so we one minus everything at the end.

For all non surface touching edging, we create a fresnel. Like before we normalise the sum of the camera's position from the the vertex's world position. We get the dot product of that passing in the world normal vector as the other argument. Using an absolute function we make everything positive, as next we one minus it to get the edge effect. We'll want a property to control the fresnel falloff so we smoothstep this into a range of 0-1 with a one minus'd float as that property and 1.

The way these two effects are merged use a maximum blending method which will always take the brightest value being output rather than an add or multiply.

---

When I was working on Sea of Thieves, non ocean (Fast Fourier transform) water uses a similar setup. We encountered a problem with water sorting between a lake going through a waterfall into a sunken cave. When water materials were overlapping, the Z-depth wasn't being used to instruct which material draws in front, so the interior water of the cave was being drawn on top of the waterfall. My first attempt was to use an opaque version to prevent any clashing. Whilst this did work the water appeared too oil like / muddy so it wasn't a visually good solution. Afterwards, we introduced depth sorting layers to guide the layering using 2 bespoke materials for that one area specifically.
