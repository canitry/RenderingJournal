
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
## 2 Ray-Surface Intersection {#ray-surface}
### 2.1 Triangle
### 2.2 Sphere
### 2.3 Transformations {#transformation}
## 3 Lighting and Shadows {#light-shadow}
### 3.1 Blinn-Phong Shading {#blinn-phong}
### 3.1.1 Ambient and Emission
### 3.1.2 Diffuse and Specular with Shadows {#shadow-ray}
### 3.2 Mirror Reflection via Recursive Raytracing {#reflection}
## 4 Acceleration Structure: OptiX Bounding Volume Hierarchy {#bvh-optix}
