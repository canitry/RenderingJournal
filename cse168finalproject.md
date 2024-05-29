# CSE 168 Final Project: Precomputed Radiance Transfer and Relighting

For the final project, I aim to implement pre-computed radiance transfer and relighting to allow for real-time rendering of scenes with changing lighting on top of the pathtracer which has been developed throughout the course. It will implement the work of Ng et al. 03, "All-Frequency Shadows Using Non-linear Wavelet Lighting Approximation". I will be doing image relighting, however, if time permits,  I will try tackling geometry relighting.

## Preliminary Results

I have integrated an IMGUI interface through which I can adjust the light intensities of the directional lights of the scene. 

<video src="FinalProject/DirectionalLightInterface.mp4" width="640" height="480" controls></video>

Multiple directional lights can be controlled with the interface, as can be seen below, and the interface will adjust to the increasing number of lights:

<video src="FinalProject/multipleDirLightInterfaces.mp4" width="640" height="480" controls></video>

Parsing for the command, `precomp`, in the `.test` files has been added to `SceneLoader` (default is off). It functions as a command to determine whether there should be precomputation and relighting for the scene or not, and whether the precomputation is for directional lights or for an environment map.

The code for the pathtracer has been refactored to no longer do progressive rendering.

Basic relighting from a few directional lights has been implemented as can be viewed in the demo video below. A new PRT specific OptiX context has been created, which takes buffers of the precomputed lighting and the directional lights and combines the precomputed lighting, weighted by the intensities of the directional lights, to relight the image each frame. The `Renderer::run()` has been changed to handle the switching between the PRT context and the OptiX pathtracer context. Changing the directional lights is now much snappier.

<video src="FinalProject/basicRelight.mp4" width="640" height="480" controls></video>

For simplicities sake, the demo above uses two directional lights encoded in the scene file. However, there is also a command one can include in the scene file, `autoLight` with parameter `dlight` and an unsigned integer `n` to create `n` directional lights whose directions are distributed (relatively) uniformly around the unit sphere. A demo of this feature can be seen below:

![](FinalProject/manyLightDemo)

Directional lights have been implemented in the direct lighting and path-tracer implementations as specified in the UCSD Online extra credit for the previous assignments.

![](FinalProject/pointandDirLight)

The integration of the basic relighting system with the point light and directional light implementations for the direct lighting system and the path-tracer system has been tested. No obvious signs of something going wrong are visible.

![](FinalProject/integrationtest)

Together, we have:

![](FinalProject/ProposalDemo)

## Next Steps

The next steps of the project are to tackle Image-Based Lighting. Then, to wavelet transform the transport matrix along its rows to allow for real time performance with higher resolution environment maps. Tackling Image-Based Lighting will mean that I will implement environment maps, namely cube maps, and add a method of rotating the image projected onto the cube map to the existing IMGUI interface so that it is simple to relight in real time. I am also aiming to implement an interface where, when environment maps are used, we can choose from a collection of environment maps.

<!-- ![Hero Video](FinalProject/heroVideo)

## Basic Relighting from a few Point Lights

## Image-Based Lighting

## Wavelet/Other Transforms -->
