---
title: "Video Player Prefabs in Unity"
date: 2022-02-22T18:22:14-05:00
draft: false
---

## TL;DR ##
- If you are instantiating multiple [video players](https://docs.unity3d.com/ScriptReference/Video.VideoPlayer.html) in Unity as Prefab objects, make sure you generate their respective `RenderTexture`'s at runtime.
   - [RenderTexture API](https://docs.unity3d.com/ScriptReference/RenderTexture.html)
- Multiple `VideoPlayer` objects rendering onto the same texture may cause issues in use-cases where you need individual `VideoPlayer` objects to respond differently.

## What Happened ##

I recently had to build for the use-case where a single scene in Unity would contain multiple video players. Each of these video players had to play/pause a video based on the position of the player relative to the respective `VideoPlayer` object [^1].

Being the Unity n00b I am, I started small and worked with a regular VideoPlayer that displayed the video on a `RawImage`. The `VideoPlayer` and `RawImage` were rendering onto a `RenderTexture`. Once I had the spatial components working, I created a Prefab out of this structure[^2] and instantiated multiple copies of it onto a test scene. (Somewhat) unbenownst to me, all these copies were referencing the exact same `RenderTexture`.

### The Bug ###
With the exception of one, none of the prefabs were responding to the player's position relative position. It's clear that they were _recognizing_ the player's position, and the respective condition was being triggered - somehow the `VideoPlayer.Play()` in that condition was not responding. 

The aforementioned exception was the first prefab in the object hierarchy. If the player went near that one prefab, the videos on all the prefabs in the scene would start playing - the relative distance to those other prefabs notwithstanding.

### The Diagnosis ###
I manually assigned new and different `RenderTexture`'s to the `VideoPlayer` and `RawImage` objects in each prefab instance. The individual prefabs were now responsive to the player's relative position without impacting the behaviour of other prefabs in the scene.

### The Fix ###
Ultimately the long-term fix is to generate new `RenderTexture` components for each such prefab and assign it to the prefab's respective VideoPlayer and RawImage. This can be done in-script using the [RenderTexture API](https://docs.unity3d.com/ScriptReference/RenderTexture.html). Some basic code may look like: 

```
RenderTexture rt = new RenderTexture(1920, 1080, 24);
VideoPlayer currVp = GetComponentInChildren<VideoPlayer>();
RawImage currRawImage = GetComponentInChildren<RawImage>();
currVp.targetTexture = rt;
currRawImage.texture = rt;
```
The above fragment will assign a new `RenderTexture` to each video player surface.

[^1]: A video would play only if the player was within a certain range of the video player. If said player moved out of range, the video would pause.  
[^2]: Insert details on the structure itself
