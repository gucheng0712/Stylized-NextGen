# Stylized-NextGen (In Development)
A stylized shader pipeline that integrating stylization into physically based rendering.
![image-20220721114614519](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207211952437.png)


## Shader Features
Listing some shader functionalities


### Perspective Correction
Modifying the Projection Matrix in the vertex shader to translate the Perspective Projection  to Orthographic Projection on specific materials based on a threshold. so the perspective removed object will be able to fit in normal perspective environment. Nice to have on  anime/stylized rendering.
![image-20220720223706612](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207202237607.png)


### Stylized Rim
Standard: Inverted Fresnel with offset based on the light direction
Screen Space: screen space depth offset based on the view space light direction.
![image-20220721204852268](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212048269.png)


### Stylized Diffuse Shading
Bands based ramp
Mihoyo Anime CG Rendering Layered Ramp Texture 
Cel Shading
![image-20220721201158465](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212012190.png)


### Stylized Specular Shading
Blinn Phong Specular with smoothstep, correctly remapped with roughness and metallic parameter
![image-20220721231600723](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212316951.png)


### Isotropic/Anisotropic/Silk
Direct Lighting:
[[2018] Call Of Duty: WWII Multi Scattering Diffuse BRDF](https://advances.realtimerendering.com/s2018/MaterialAdvancesInWWII.pdf)
GGX Normal Distribution Function
Smith GGX Visibility Term 
![image-20220721110211816](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207211952109.png)


### Fabric
[Charlie Normal Distribution Function](https://knarkowicz.wordpress.com/2018/01/04/cloth-shading/)
Ashikhmin Visibility Term


### Hair
[Energy-Conserving Wrapped Cosine Lobe Diffuse](http://blog.stevemcauley.com/2013/01/30/extension-to-energy-conserving-wrapped-diffuse/)
Two Layer KajiyaKay Strand Specular
Tangent Map 


#### Environment Mapping
Isotropic Environment Mapping: Unity built-in approximation on UE split sum DFG LUT

Anisotropic Environment Mapping: using anisotropic modified normal as reflection vector

Cloth Environment Mapping: split sum DFG LUT for cloth
![img](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212327336.png)

Refraction/Reflection

Custom CubeMap/ SphericalMap

Toony Interpolation



### Thin Film (Iridescence)
Modified Iridescence BSDF
Reference: https://belcour.github.io/blog/research/publication/2017/05/01/brdf-thin-film.html

![image-20220720235334319](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207210013411.png)

### Clear Coat

Modified clearcoat BSDF

![image-20220721200730373](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212007495.png)


### Hair Shadow

**Screen Space Hair Shadow(Render Feature):** 

Use RenderFeature to generate the RT of a hair depth Pass (use Layer Mask to determine whether it is a hair part), and then use a uv pair in the lighting Pass to make an offset value according to the lighting direction It samples and compares it with the character's current depth, and the difference is the shadow of the hair.

**Hand painted shadow with view space light direction UV offset :**

Using UV offset to achieve hair shadows that change according to the direction of the light is also a better solution, but it is much better in performance than screen space hair shadows.

![image-20220721202424374](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212024584.png)


### Skinned GPU Instancing Fur Shell

Unity exposed SkinnedMeshRenderer.GetVertexBuffer() API since Unity 2021.2.0, so that it is possible to get skinned VBO, and then use ByteAddressBuffer to pass it to Shader (because DX11 only allows Raw to pass VBO/IBO) by using DrawProcedural() in RenderFeature to achieve Skinned GPU Instancing.

![image-20220721205513837](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212055619.png)


### SDF Shadow

Created by Collaborator



### Subsurface Scattering
![image-20220721205221614](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212052523.png)
![PerspectiveRemoval](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207210008354.gif)

Modified SSS from [GDCVault: Approximating Translucency for a Fast, Cheap and Convincing Subsurface Scattering Look](https://www.gdcvault.com/play/1014538/Approximating-Translucency-for-a-Fast) 

Modifications:

* Add a custom surface depth param into the back lit calculation,

* Add some modified Fresnel like equation on both front and back lit.

* Parallax Refraction





### Matcap

Modified Matcap calculation to reduce to stretch at the viewing edge area

![image-20220721001326669](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207210013685.png)


### Outline

* View Space FOV applied constant width outline
* light space outline

### Dither Fade
* Bayer 4x4
* Bayer 8x8
* Custom Dither Texture

![dithering](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202206170015011.gif)


### Parallax Mapping
Some General Parallax Mapping Techniques: Used in Eye, Gem, Bump Materials

* Parallax Mapping
* Layered Parallax Mapping
* Relief Mapping
* Parallax Occlusion Mapping
![image-20220721200512845](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212005736.png)


### Triplanar Projection
* Height Blending
* Object Space / World Space
* Normal Blending
![image-20220721233440390](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212334280.png)



### TopDown Projection
* Height&Normal Angle Blending
* World Space Only
![image-20220721233653848](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207212336484.png)


## Unity Pipeline Tools

### Generic Shader GUI

A general Shader GUI Framework in Unity, written in order to write minimal code and keep it flexible and easy to use.  The combination of Unity Material Property Drawer API and additional regex based shader file analyzation.

![image-20220720003232299](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207200032313.png)

### Smooth Normal Baker

Using FBX SDK to modify fbx file to bake the smoothed normal into the vertex color or UV Channels for fixing outline breaking issue

![image-20220721163850689](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207211952191.png)

### Face SDF Generator

![image-20220721164452176](https://raw.githubusercontent.com/gucheng0712/ImageHoster/main/src/202207211952704.png)

### Custom Post Processing Framework

Based on URP RenderFeature, A buffer swapped postprocessing framework. Be able to insert custom post processing in custom render event.

### Shader Generator

Generating shader based on the features used. It will strip all shader variants and replace with constant value. 

## DCC Pipeline Tools

### Fur Shell Generator

A simple tool created in maya for batching furshell creation process

Shell Generation Process:
1. Duplicating Selected Mesh based on the assigned layer amount.
2. Vertex Painting on each Mesh based on the current layer index.
3. Combine these duplicated meshes, and cleanup history and scene.
4. Find the Root joint and bind it to the mesh skin through skin cluster.
5. Copy the skin weight from the source skin cluster to the combined skin cluster.
6. Delete source mesh



