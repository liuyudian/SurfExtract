# SurfExtract
The application extracts a point cloud from all visible surfaces of a mesh model; it ignores all hidden surfaces. 
The tool renders the surface of a 3D model from multiple locations around a sphere into a 32bit fbo. 
The pixels contain just model positions and a point for each valid pixel is created. 

![Figure 1: Overview working principle: The tool renders surface data from multiple locations into a 32 bit fbo. The data is sampled to generated point clouds. The point clouds are merged and a uniform voxel grid prevents duplicates.](https://github.com/rafael-radkowski/SurfExtract/blob/master/data/media/Overview.jpeg)
Figure 1: Overview working principle: The tool renders surface data from multiple locations into a 32 bit fbo. The data is sampled to generated point clouds. The point clouds are merged and a uniform voxel grid prevents duplicates.

Subsequent sampling prevent duplicates and also allows one to reduce the point cloud density via a voxel grid. 
In addition, the application also calcualtes surface normal vectors as a cross product and writes them into the obj file. 

![](https://github.com/rafael-radkowski/SurfExtract/blob/master/data/media/PointCloudSampling.jpeg )

The latest version of the software also calculates normal vectors as the figure below shows. 
All vectors are normalized; the viewer renders them quite short so that the model remains visile. 
The normal vectors - its renderer - can be enabled by pressing '3' after the surface point extraction is done.

![](https://github.com/rafael-radkowski/SurfExtract/blob/master/data/media/normalvectors.png )


### Prerequisites
All code runs (tested) on Windows 10, Version 1803. It was developed with Visual Studio 2017.
It requires the following 3rd party tools:
 * [OpenCV v3.4.5](https://opencv.org)
 * [Eigen3 v3.3.7](http://eigen.tuxfamily.org)
 * [GLEW v2.1.0](http://glew.sourceforge.net)
 * [GLFW v3.2.1](https://www.glfw.org)
 * [glm, v0.9.9.3](https://glm.g-truc.net/0.9.9/index.html)
 
Additinally, all code from my OpenGL utils folder is required:
 * [OpenGL Helpers](https://github.com/rafael-radkowski/GLSupport)

### Usage
Use command line arguments to configure the application; syntax:
```
SurfExtract.exe [input_path_and_file.obj] -o [output_path_and_file.obj] -c [camera_distance] -d [voxel_grid_distance] -s [uniform_scale]
```
with
* -o the output path and file. Currently, only .obj files are written.
* -c the camera distance. Find a distance that covers your model size. 
* -d uniform voxel grid size. This values determines the number of points. 
* -s set a uniform scale parameter between 0.0001 < s < 10000.0 to scale the final point cloud (optional), default = 1.0

Example:
```
 SurfExtract.exe ../data/models/Rubber_Duck_02.obj -o point_cloud.obj  -c 4.5 -d 0.025
```

### Known issues and shortcommings
* The .obj model input requires an mtl material file. Otherwise, the obj loader does not process the file. Create an empty one if you do not have one.
* Mind your model size. The default camera is quite close. So if your model vertex values are huge, you might end up inside the model. This will be changed in future. 
* Some error messages appear at the beginning. IGNORE THEM.


### Binary Versions
A binary version pre-compiled for Windows 10 is available on DropBox
#### Version V1.0-190425
* [Link](https://www.dropbox.com/sh/npenx0m40lyjx46/AAA3Gpv0HK-AxPzqTPi0U4DAa?dl=0&lst=)
* Comes with all basic features as described above. 

### Version history
V 1.1.3
- moved all shader code to cpp files. 
- automatic centroid detection
- calculate camera distance depending on the size of the object. 
