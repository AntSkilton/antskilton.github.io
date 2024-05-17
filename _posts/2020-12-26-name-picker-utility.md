---
layout: single
title:  "Name Picker Utility"
excerpt: "A small utility for a big Olympic event!"
categories: [tool]
toc: true
---
## The Requirement

I went on holiday this year with my friends and we were planning a series of mini sporting events in our villa based around a version of our own Olympics *(yeah we took it seriously, there was a trophy involved)*. We were all competing, sometimes in pairs, sometimes as individuals. We needed a utility without bias to provide these random match made assignments. Even though this didn't warrant a game engine, I stuck to what I knew for deployment to Android. Here it is in Unity.

![image-center](\assets\images\2020-12-26-name-picker-utility\namePicker.gif "Finished result"){: .align-center}

## C#

It uses delegates and events via the observer patter hooked up to the UI and all comes from one script. The names are hardcoded and primed into an available names list on `Start()`, with an empty chosen names list instantiated too. The main function below moves the entries about accordingly.

```
    public void NamePick()
    {
        if (SquadNames.Count!=0){
            int randIndex = Random.Range(0, SquadNames.Count);
            ChosenNames.Add(SquadNames[randIndex]);
            ChosenNamesText.SetText(string.Join(", ", ChosenNames));
           
            SquadNames.RemoveAt(randIndex);
            NamesAvailableText.SetText(string.Join(", ", SquadNames));  
        }

        else{
            Debug.LogError("List is empty. Reset names.");
            return;
        }
    }
```

The reset name button then clears both lists and initiates the `Prime()` function again giving it a clean reliable reset. If I were to extend this, user input for custom names and a better layout for round robin configurations would be a good place to start.
