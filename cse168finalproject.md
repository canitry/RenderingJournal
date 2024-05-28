# CSE 168 Final Project: Precomputed Radiance Transfer and Relighting

For the final project, I aim to implement pre-computed radiance transfer and relighting to allow for real-time rendering of scenes with changing lighting on top of the pathtracer which has been developed throughout the course. It will implement the work of Ng et al. 03, "All-Frequency Shadows Using Non-linear Wavelet Lighting Approximation". If time permits,  I will try tackling changing the camera position within the scene.

For the preliminary results: 

I have integrated an IMGUI interface through which I can adjust the light intensities of the scene. 

![](FinalProject/interfaceImage)

Parsing for `precomp on` has been added to `SceneLoader` (default is off) as a command to add to `.test` files to determine whether there should be precomputation and relighting for the scene or not.

I have also implemented basic relighting from a few point lights as can be viewed in the demo video below.

![](FinalProject/basicRelight)

Point lights and directional lights have been implemented in the direct lighting and path-tracer implementations as specified in the UCSD Online extra credit for the previous assignments.

![](FinalProject/pointandDirLight)

The integration of the basic relighting system with the point light and directional light implementations for the direct lighting system and the path-tracer system has been tested. No obvious signs of something going wrong are visible.

![](FinalProject/integrationtest)

Together, we have:

![](FinalProject/ProposalDemo)

The next steps of the project would be to tackle Image-Based Lighting. Then, to wavelet transform the transport matrix along its rows to allow for real time performance with higher resolution environment maps. Tackling Image-Based Lighting will mean that I will implement environment maps, particularly cube maps, and, adding to the existing IMGUI interface, a method of rotating the image projected onto the cube map so that it is simple to relight in real time.

![Hero Video](FinalProject/heroVideo)

## Basic Relighting from a few Point Lights

## Image-Based Lighting

## Wavelet/Other Transforms
