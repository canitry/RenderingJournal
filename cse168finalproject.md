# CSE 168 Final Project: Precomputed Radiance Transfer and Relighting

This project implements pre-computed radiance transfer and relighting to allow for real-time rendering of scenes with changing lighting on top of the previously developed. It implements Ng et al. 03, "All-Frequency Shadows Using Non-linear Wavelet Lighting Approximation".

## User Interface for Lighting

I have integrated a DearImGui interface through which I can adjust the light intensities of the directional lights, point lights and area lights of the scene. 

<video src="FinalProject/DirectionalLightInterface.mp4" width="640" height="480" controls></video>

<video src="FinalProject/pointLightDemo.mp4" width="640" height="480" controls></video>

<video src="FinalProject/areaLightDemo.mp4" width="640" height="480" controls></video>

Multiple lights can be controlled with the interface, as can be seen below, and the interface will adjust to the increasing number of lights:

<video src="FinalProject/multipleDirLightInterfaces.mp4" width="640" height="480" controls></video>

### To Do

Further commands for selecting different environment maps (once they have been implemented) and rotating the image projected onto the cube map to the existing ImGui interface so that it is simple to relight in real time.

## Basic Relighting from a few Point Lights

Parsing for the command, `precomp`, in the `.test` files has been added to `SceneLoader` (default is off). It functions as a command to determine whether there should be precomputation and relighting for the scene or not, and whether the precomputation is for directional lights or for an environment map.

The code for the pathtracer has been refactored to no longer do progressive rendering.

Basic relighting from a few directional lights has been implemented as can be viewed in the demo video below. A new PRT specific OptiX context has been created, which takes buffers of the precomputed lighting and the directional lights and combines the precomputed lighting, weighted by the intensities of the directional lights, to relight the image each frame. The `Renderer::run()` has been changed to handle the switching between the PRT context and the OptiX pathtracer context. Changing the directional lights is now much snappier. The previous demos of the area light and point light interface also show that relighting works for area lights and point lights

<video src="FinalProject/basicRelight.mp4" width="640" height="480" controls></video>

<video src="FinalProject/pointLightDemo.mp4" width="640" height="480" controls></video>

<video src="FinalProject/areaLightDemo.mp4" width="640" height="480" controls></video>

In the demo, it simply uses the lights in the scene file, however, another command one can include in the scene file, `autoLight` with an integer parameter `n` will create `n` directional lights whose directions are distributed (relatively) uniformly around the unit sphere using the Fibbonacci sphere algorithm detailed here: ["Evenly distributing points on a sphere" by Martin Roberts](https://extremelearning.com.au/evenly-distributing-points-on-a-sphere/)

<video src="FinalProject/dLightGen.mp4" width="640" height="480" controls></video>

### To Do

I will also implement directional lights in the direct lighting and path-tracer implementations as specified in the UCSD Online extra credit for the previous assignments.

## Image-Based Lighting

### To Do

Image-based lighting has not been implemented yet.  

The steps of implementing this feature will be

1. Environment Maps (Cube Maps)
2. Commands for PRT for environment maps.

## Wavelet/Other Transforms

### To Do

Wavelet transforming the transport matrix along the rows has not been implemented yet.

I will use the simple Haar wavelet for this.
