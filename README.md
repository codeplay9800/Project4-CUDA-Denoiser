CUDA Denoiser For CUDA Path Tracer
================

**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 4**

* Shubham Sharma
  * [LinkedIn](www.linkedin.com/in/codeshubham), [personal website](https://shubhvr.com/).
* Tested on: Windows 10, i7-9750H @ 2.26GHz, 16GB, GTX 1660ti 6GB (Personal Computer).
*GPU Compute Capability: 7.5


## Project Overview
In this project, I implemented a pathtracing denoiser based on "Edge-Avoiding A-Trous Wavelet Transform for fast Global Illumination Filtering" by Dammertz, Sewtz, Hanika, and Lensch. The Denoiser uses geometry buffers (G-buffers) which is convoluted with the original Ray Traced output to provide a smooth result  and therby acting as smoothing filter.

## Algorithm Overview
- The input is a path-traced image,
![Performance  Analysis](img/Outputs/10iter.PNG)

- Normal buffer 
![Performance  Analysis](img/normals.png)

- Position buffer
![Performance  Analysis](img/positions.png)

The algorithm uses Gaussian filter with increasing step width for multiple iterations to get a denoised result.

# Performance Analysis
The following figures show the renders with different denoising iterations.

All the Images Use The Following Parameters 
- Filter Size = 25
- Color Weight = 8.1000
- Normal Weight = 0.350
- Position Weight = 0.700

## Reference Image (1000 Iterations) 
 
## 10 Iterations 
- Reference
![Performance  Analysis](img/Outputs/10iter.PNG)

- Denoised
![Performance  Analysis](img/Outputs/denoised.PNG)

## 2 Iterations
- Reference
![Performance  Analysis](img/Outputs/2iter.PNG)

- Denoised
![Performance  Analysis](img/Outputs/denoised2.PNG)

## Performance Numbers


### Cost Denoiser vs Path Trace

- Filter Size = 80
- Color Weight = 8.1000
- Normal Weight = 0.350
- Position Weight = 0.700

![Performance  Analysis](img/Outputs/PathTracerandDenoiser.png)

### Denoiser vs Resolution

- Filter Size = 80
- Color Weight = 8.1000
- Normal Weight = 0.350
- Position Weight = 0.700

![Performance  Analysis](img/Outputs/DenoiseTimevsResolution.png)

This demonstrates that the average time increases linearly with the number of pixels in the image. 

### Denoiser vs Filter Size
![Performance  Analysis](img/Outputs/DenoiseTimevsFilterSize.png)

The À-Trous filter scales in power of 2 which aligns with the demonstration of this graph curve which behaves as a logarithmic curve. 

### Notable Findings
- How visual results vary with filter size.
-   - In my Findings the visual results do not change linearly with filter size. As the filter size increase the difference in results become less percieveable 

- how do results compare across different scenes - for example, between cornell.txt and cornell_ceiling_light.txt. Does one scene produce better denoised results? Why or why not?
- - A well Lighted scene will have 


## Optimisations

### G-Buffer optimization
- Optimising per-pixel positions and normals which are stored as glm::vec3s results in smaller storage buffers size and access times. You can see the performance improvements from charts and numebrs below
- - positions are now reconstructed based on pixel coordinates using an inverted projection matrix.
- - Normals are compacted from glm::vec3 to glm::vec2 saving 4byte per float value stored.

- Optimised Charts
![Performance  Analysis](img/Outputs/PathTracerandDenoiser.png)

Trial #	1	2	3	4<br/>
Path Tracer	80.135ms	82.127ms	84.653ms	85.317ms<br/>
Denoiser	13.0652ms	13.157ms	13.265ms	13.59ms<br/>

- UnOptimised Charts
![Performance  Analysis](img/Outputs/PathTracerandDenoiserUnoptimised.png)

Trial #	1	2	3	4<br/>
Path Tracer	87.6103ms	89.3156ms	90.134ms	90.9634ms<br/>
Denoiser	9.5657ms	9.7115ms	9.7557ms	9.7799ms<br/>
### Comparing A-trous and Gaussian filtering

- A-trous
![Performance  Analysis](img/Outputs/denoised.PNG)

- Simple Gussian Blur 
![Performance  Analysis](img/Outputs/Gaussian.PNG)


As we can see the A-tours filter does a much better job in preserving the edges which almost seem too fade in simple Gaussian Blur. 

| Filter Size | A Trous   |  Simple Gaussian | 
|---|---|---|
| 25 |  8.5991	 |  1.3322 |
| 80 |  13.590	 | 3.1142  |

