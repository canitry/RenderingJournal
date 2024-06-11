# CSE 168 Final Project: Precomputed Radiance Transfer and Relighting

This project implements pre-computed radiance transfer and relighting to allow for real-time rendering of scenes with changing lighting on top of the previously developed. It is based on Ng et al. 03, "All-Frequency Shadows Using Non-linear Wavelet Lighting Approximation".

## User Interface for Lighting

I have integrated a DearImGui interface through which I can adjust the light intensities of the directional lights, point lights and area lights of the scene. 

<video src="FinalProject/DirectionalLightInterface.mp4" width="640" height="480" controls></video>

<video src="FinalProject/pointLightDemo.mp4" width="640" height="480" controls></video>

<video src="FinalProject/areaLightDemo.mp4" width="640" height="480" controls></video>

Multiple lights can be controlled with the interface, as can be seen below, and the interface will adjust to the increasing number of lights:

<video src="FinalProject/multipleDirLightInterfaces.mp4" width="640" height="480" controls></video>

There is also an interface for adjusting the rotation of the environment map via axis-angle style of controls. The axis can be controlled by adjusting the phi and theta of the axis vector.

<video src="FinalProject/specFullWavelet.mp4" width="640" height="480" controls></video>

## Basic Relighting from a few Point Lights

Parsing for the command, `precomp`, in the `.test` files has been added to `SceneLoader` (default is off). It functions as a command to determine whether there should be precomputation and relighting for the scene or not, and whether the precomputation has no environment map (`precomp dlight`) or with an environment map (`precomp envmaps`).

The code for the pathtracer has been refactored to no longer do progressive rendering.

Basic relighting from a few directional lights has been implemented as can be viewed in the demo video below. A new PRT specific OptiX context has been created, which takes buffers of the precomputed lighting and the directional lights and combines the precomputed lighting, weighted by the intensities of the directional lights, to relight the image each frame. The `Renderer::run()` has been changed to handle the switching between the PRT context and the OptiX pathtracer context. Changing the directional lights is now much snappier. The previous demos of the area light and point light interface also show that relighting works for area lights and point lights

<video src="FinalProject/basicRelight.mp4" width="640" height="480" controls></video>

<video src="FinalProject/pointLightDemo.mp4" width="640" height="480" controls></video>

<video src="FinalProject/areaLightDemo.mp4" width="640" height="480" controls></video>

In the demo, it simply uses the lights in the scene file, however, another command one can include in the scene file, `autoLight` with an integer parameter `n` will create `n` directional lights whose directions are distributed (relatively) uniformly around the unit sphere using the Fibbonacci sphere algorithm detailed here: ["Evenly distributing points on a sphere" by Martin Roberts](https://extremelearning.com.au/evenly-distributing-points-on-a-sphere/)

<video src="FinalProject/dLightGen.mp4" width="640" height="480" controls></video>

## Image-Based Lighting

We can have an environment map used in the scene with the command `precomp envmap`. For the environment map, we use an `.hdr` cube map with faces of resolution 256 x 256 is placed inside the project itself where the executable is located. However, the texels of the environment map itself within the scene is a downsampled version of the cube map, with 16 x 16 texels per face.

Precomputed rendering for the environment map is done in the following manner:

During precomputation, for each texel of the environment map, we render one image of the scene wherein there is effectively only one point of illumination, the texel set to a value of `(1.0f, 1.0f, 1.0f)`.

These texels light the scene through the miss() program, wherein the vector of the ray which misses all objects in the scene is converted to a texel coordinate and compared with the inputted texel coordinate.

This values are stored (and operated on if the command `wavelet` is used) to form the transport matrix.

Then, during rendering, each texel is converted back to the rotation vector they are associated with, rotated using the axis-angle inputted from the interface, then converted to a uv for the appropriate face then sampled from the 256x256 texture. Texture sampling the original hdr image is done through the TextureSampler in OptiX and the HDRLoader located in the `utils` of the [OptiX CSE 168 Starter Code](https://github.com/CSE168sp20/CSE-168-OptiX-Starter-Code). The resulting color is recorded for each texel. To do this, a new OptiX Context (`lsContext`) has been created, which takes the number of texels and the axis and angle of rotation and outputs the sampled lights. This buffer is then converted to a vector (and operated on if the command `wavelet` is used) to form the light vector.

Finally, when the PRT context is launched, transport matrix and the light vector for the environment map is passed to the PRT context, wherein the two matrixes are effectively matrix multiplied together and then added to the result for the non-environment map lights.

<video src="FinalProject/EnvMapDemo.mp4" width="640" height="480" controls></video>

<video src="FinalProject/manyEnvMap.mp4" width="640" height="480" controls></video>

## Wavelet

The wavelet transform can be specified by the `wavelet` command. This will wavelet transform the rows of the environment map transport matrix.

More specifically, this will do the 2D Haar Wavelet transform onto the precomputed lighting values for each face of texels for each pixel. This is done by, for each face, taking the precomputed images of each texel (which can be thought of as a matrix with the lighting values per each texel in the face as the columns and the pixel values as the rows) then flipping the matrix quickly through launching an optix context (`flipContext`) so that we have the lighting values for each pixels as the rows and the pixels as the columns. This buffer is then passed to a while loop, which continuously passes the buffer to an OptiX context (`haarContext`) which performs one step of the 2D haar wavelet transform onto the lighting values for each pixel (treating the lighting values as a 16 x 16 matrix) until the haar transform is complete. The collected values are passed as transport matrix.

This same process is applied to the lighting vector as well, for the 6 faces.

During the PRT Context, we do the same process as non-wavelet environment lighting. We get the same result as without wavelets:

<video src="FinalProject/fullwavelet.mp4" width="640" height="480" controls></video>

<video src="FinalProject/specFullWavelet.mp4" width="640" height="480" controls></video>

### First 100 Terms

However, if one uses the command `sparsify` (the name is not reflective of the operation), only the first 100 terms per each face are used. This is done by, when appending values of the buffer of haar transformed values to our vector of stored values, we only take the first 100 values per face (so for each pixel there are 600 values). The result is a coarser estimation but a more responsive render.

<video src="FinalProject/first100terms.mp4" width="640" height="480" controls></video>

<video src="FinalProject/spec100terms.mp4" width="640" height="480" controls></video>

### Further Considerations

Making this more memory efficient, such as through the sparsified matrix would be a good next step. Further, it may be worthwhile to record the values for all texels/lights for a single pixel per step of precomputation rather than an image, as this would allow us to avoid having to flip the buffer and may make the values for the precomputed values for environment maps cleaner.