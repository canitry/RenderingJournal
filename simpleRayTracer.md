
# Simple Ray Tracer

A RayTracer that renders triangles and spheres and ellipsoids with Blinn-Phong shading, mirror reflections, transformation stack, and a BVH acceleration structure based on the skeleton code and tutorial from the [CSE 168 OptiX Tutorial](https://github.com/CSE168sp20/CSE-168-OptiX-Tutorial/tree/master).  

This assignment is created for Assignment 1 of the CSE 168 Spring 2024 course at UCSD with Ravi Ramamoorthi.

![](SimpleRayTracer/scene4-ambientWM.png)
![](SimpleRayTracer/scene4-diffuseWM.png)
![](SimpleRayTracer/scene4-emissionWM.png)
![](SimpleRayTracer/scene4-specularWM.png)
![](SimpleRayTracer/scene5WM.png)
![](SimpleRayTracer/scene6WM.png)
![](SimpleRayTracer/scene7WM.png)

## 1 Ray Generation {#ray-gen}

Ray Tracing
![](SimpleRayTracer/pinhole.png)
Rays are shot through the center of the pixels to approximate the amount and quality that hits the entire area of the pixel. The camera or eye, like a pinhole camera, has a point where all light rays converge  Consider a pinhole camera with its position (lookfrom), the direction it is looking at (lookat), the direction the top of the camera is (up), and fov.
While multiple 
We then approximate the amount and quality of the light that hits each pixel. 
These This is because there should be one ray per pixel, and each ray should come from the center of the pixel. The `.test` file, which we parse to acquire information about the sceneCamera position (also termed "lookFrom"), the direction the center of the camera is looking (also termed "lookAt"), the fov in the y direction, and 
$$ $$
(Note: in the `.test` file provided, the fov y is in degrees, not radians, not knowing this left me with some headscratching for a while, you simply convert to radians while parsing.)

## 2 Ray-Surface Intersection {#ray-surface}
### 2.1 Triangle
### 2.2 Sphere
### 2.3 Transformations {#transformation}
## 3 Lighting and Shadows {#light-shadow}
### 3.1 Blinn-Phong Shading {#blinn-phong}

This ray tracer is based off the idea of sight as the result of light, which has been emitted from a source and bounced off and affected by the material of that object, reaching our eye. The ray tracer, however, reverses the direction, and traces from the eye to the objects to the light (or traces only up to a certain amount and eventually stops and approximates the rest), to approximate the resulting color and intensity from the potential paths of rays of light that reached the eye.  

One might imagine there are more and less convoluted ways for light to reach the eye. It might directly reach the eye from the light source, or perhaps hit one object then directly hit the eye, or further hit a number of different objects before hitting the eye. Blinn-Phong Shading approximates the shading  

### 3.1.1 Ambient and Emission
### 3.1.2 Diffuse and Specular with Shadows {#shadow-ray}
### 3.2 Mirror Reflection via Recursive Raytracing {#reflection}
## 4 Acceleration Structure: OptiX Bounding Volume Hierarchy {#bvh-optix}
