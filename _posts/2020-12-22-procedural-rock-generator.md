---
layout: single
title:  "Procedural Rock Generator"
excerpt: "My process for creating procedural rocks using Houdini & Substance Designer."
categories: procedural-modelling houdini substance-designer
toc: true
---
![Alt](\assets\images\2020-12-22-procedural-rock-generator\comp1.jpg)

![Alt](\assets\images\2020-12-22-procedural-rock-generator\comp2.jpg)

Procedural rock creation in Houdini was something I wanted to explore due to rocks being a common time sink in game production, but usually bare the same rules. I've managed to automate the high and low pipeline with the input being a somewhat primitive shape. The beauty of this tool/process is that whatever shape that is required, it can be created to suit the environment in a short amount of time, with visual consistency to every other rock.

## High Level Process

There are a few expressions and a little bit of VEX involved but this is the high-level process.

- Start with a box based primitive
- Voronoi shatter and create chunks
- For each loop the centroid transform of each prim for a funky silhouette
- Voxelise mesh to remove interior unneeded geometry
- Break up the sharp silhouette with large noise using a mountain and a custom noise VOP
- Smoothen out clipping geometry
- Blast away floating chunks (if any - not needed for my simple input prims)
- Revoxelise to a tighter degree ready for another noise pass
- Higher fidelity noise pass. With a final sub-division, export the high poly
- For the low poly, remesh it, poly reduce, fuse and remove bottom base geometry
- SideFX Labs (used to be called Game Dev Toolset) saving the day with auto-uv, uv-smooth, packing, smooth by UV island and output the low poly

![Alt](\assets\images\2020-12-22-procedural-rock-generator\1.jpg)
Graph overview. Please reference this for exact node names and structure context.

## 1. Input Shapes

I assessed what shapes I needed from the concept and blocked out it with primitives. You can make these in Houdini easily with edit nodes, or pull in some blockout meshes from any DCC. The below gif shows the primitive model as the input, with the exported high baking model and runtime model. As you can see from the overview, this is the point where we can swap inputs into the rest of the algorithm.

![Alt](\assets\images\2020-12-22-procedural-rock-generator\process.gif "One primitive example"){: .align-center}

## 2. Shatter

A simple Voronoi fracture from the shelf does the heavy lifting of finding points of the fractures to input into Voronoi. Setting cut plane offset to `0.02` was important for taking the fracture spline work and creating the chunks from that. To do this manually you'd convert it to a fog, scatter points over that and then fracture based on those points.

![Alt](\assets\images\2020-12-22-procedural-rock-generator\voronoi.png)

## 3. Chunks

To create a ledge like silhouette, I rotate all the chunks of the fracture about themselves on on X and Z. First make sure that the pivot transforms ae cantered for each chunk (both translate and rotate) are set to `$CEX`, `$CEY` and `$CEZ` respectively. This is run over a for loop for each chunk. You can adjust the variance of chunk rotation or shatter density in step 2 if you feel that the new silhouette strays too far from your blockout.

![Alt](\assets\images\2020-12-22-procedural-rock-generator\chunkArrangement.png)

The surface is intersecting and suboptimal, so we need to clean that up by voxelising it using the `vdbfrompolygons` node. From this voxel data date we can apply a `sop_instant_meshes` node and give it some clean high density topology. This node is requires [SideFX Labs](https://www.sidefx.com/tutorials/sidefx-labs-installation/) (version 18+).

![Alt](\assets\images\2020-12-22-procedural-rock-generator\vdb.png)

Below is this step's parameters.

![Alt](\assets\images\2020-12-22-procedural-rock-generator\2.png)

## 4. Large Noise

For noise operations, compiling on each update is resource-intensive in a VOP hence the 5 promoted parameters. This is a noise fitted to a range for each of adjustment. Once I was happy I used these settings on each rock for consistency. Playing about with various inputs such as magnitude and turbulence did a lot of the heavy lifting. We will have erratic geometric which will need fixing, but the rock texture should be there for a visual read from afar.

![Alt](\assets\images\2020-12-22-procedural-rock-generator\3.png)

## 5. Smoothing

We need to check and isolate any geometry that may be manifolded with itself due to the noise. The attribute wrangle takes in the primitive's normals and casts a ray. If that ray doesn't collide with anything, then it's not intersecting with itself. If the ray does collide however, we can tag those prims into a selection group called "intersection". The group promote converts the prims to points.

Next we use the "intersection" group and grow it to encompass a soft falloff. Then we can apply the smooth to that group only relaxing that tagged geometry. Some irregular looking pinching can appear but it removes a lot of the larger looking odd bubonic bulges which is good enough.

![Alt](\assets\images\2020-12-22-procedural-rock-generator\4.png)

## 6. Cleanup

Here is the last VDB, with a finer and smaller voxel size than before. This primer is resetting the mesh and making sure it's still concurrent. If there is any floating chunks outside of the model get removed with the blast node to the inverted selection of selecting the main mesh. Personally, this step might be overkill, but it depends on your previous parameters. Either way it's a good test to have in case.

## 7. Fine Noise

This is similar to the first noise pass but with different parameters. Again more X and Z horizontal stretching Perlin to have those those sedimentary streaks. I prefer baking these in the high poly rather than a substance world space projection because it feels more organic, as you see the lineage curve around the crests as the wind has worn about at it over time. I add in a user ramp input at this point so we can have a curve exposed on the tier above. This acts just like a curve node in Designer we can artistically control the peaks and troughs. Like before, all normalised to a range.

![Alt](\assets\images\2020-12-22-procedural-rock-generator\curve.png){: .align-center}

Export this as the high poly. I avoid more organic or finer details in Houdini, as I see it to be more appropriate to apply that layer in Substance Painter.

## 8. Retopologise

Continuing on the chain for the low poly we `remesh` it to an acceptable silhouette. `Poly reduce` creates the low poly from this so the data is more practical to work with. We create a group with a proximity box for the base of the rock. Any points in that box will get tagged in this volume will get `blasted` away as they'll no longer be required. To clean up the hole perimeter, you can create a perimeter group to select and flatten / smooth the rim with a `soft transform` so that it's cleaner to dress an environment with.

## 9. UVing

`Auto UVing`, with a `relax` does the job, also thanks for the extendable content. I found a decent island to packing ratio on the 2K maps these are loaded in at. The SideFX Labs SOP hardens edges of the mesh by UV island which is important for bespoke bake matching. The quality of auto peeling and packing isn't as good as a human even after a bit of parameter adjustment. Send it for the low poly export.

## 10. Substance

Up until this point the noise has been relatively uniform. I like to add non-uniformity in Painter which includes the porosity in the Designer material input. I have a light touch of AO for the rock chunk intersections too.  Also, some sporadic vertical Y axis based cracks to help out. Once happy I saved this as a smart material so I can apply it to all meshes with guaranteed continuity.

![Alt](\assets\images\2020-12-22-procedural-rock-generator\5.png)

## 11. Conclusion

I think Houdini plays a fantastic role in producing a large portion of the artwork. Elements such as UVing, I'd still prefer to do by hand as it'd be worth the time in tradeoff for quality. Custom world space slope shaders would be desired in engine to tie the rock to the environment, which I'd imagine would share locally shared properties such as moss, snow or mud. For the purpose of these semi realistic rocks, they are modular vista assets which serve their purpose rather well considering the scale of them in the environment to the time it'd take an artist to manually sculpt these forms. I would be dubious of using this workflow for stylised rocks.

![Alt](\assets\images\2020-12-22-procedural-rock-generator\6.png)
Here is that initial rock in Marmoset Toolbag.
