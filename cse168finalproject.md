# CSE 168 Final Project: Precomputed Radiance Transfer and Relighting

For the final project, I aim to implement pre-computed radiance transfer and relighting to allow for real-time rendering of scenes with changing lighting on top of the pathtracer which has been developed throughout the course. It will implement the work of Ng et al. 03, "All-Frequency Shadows Using Non-linear Wavelet Lighting Approximation". If time permits,  I will try tackling changing the camera position within the scene.

For the preliminary results: 

I have integrated an IMGUI interface through which I can adjust the light intensities of the scene. 

![](interfaceImage)

I have also implemented basic relighting from a few point lights as can be viewed in the demo video below.

![](interface)

The next steps of the project would be to tackle Image-Based Lighting. Then, to wavelet transform the transport matrix along its rows to allow for real time performance with higher resolution environment maps. Tackling Image-Based Lighting will mean that I will implement environment maps, particularly cube maps, and, adding to the existing IMGUI interface, a method of rotating the image projected onto the cube map so that relighting.

![Hero Video](FinalProject/heroVideo)

## Basic Relighting from a few Point Lights

## Image-Based Lighting

## Wavelet/Other Transforms
