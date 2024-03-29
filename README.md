# Blendyn Development
by Do Tien Dung, September, 2022

## Project overview and objective
[Blendyn](https://github.com/zanoni-mbdyn/blendyn) is one of post-processing tools of [MBDyn](https://www.mbdyn.org/) - multibody simulation engine. Blendyn is built as an Add-ons of [Blender](https://www.blender.org/) which allow user to visualise MBDyn output datas with 3D animations. Blendyn was first implemented in 2017. However, there are some rooms for improvement and some parts of current Blendyn was out of date. In this project, I focus on developing some of them:
1. Improving graph plotting function and adding two new plotting engines into Blendyn `matplotlib` and `bokeh` beside `pygal`
2. Visualising two MBDyn elements: `beam slider` and `modal`
3. Visualising internal force and internal moment for deformable elements: `beam2`, `beam3`, `shell4`, `membrane4`
4. Improving UI

## Code
- [Blendyn Github branch "devel2"](https://github.com/zanoni-mbdyn/blendyn/tree/devel2) 
- [File Change](https://github.com/zanoni-mbdyn/blendyn/pull/51/files)
- [Commits History](https://github.com/zanoni-mbdyn/blendyn/pull/51/commits)

## Critical Algorithms
#### Animating modal

The most challenging section for visualising `modal` element is extract `modal node` data from different files and put them together in one formula
to make the `motion paths`. The image below show how I extract and use the file data to get the modal location.
<p align = "center">
<img width="1500" alt="Modal animation" src="https://user-images.githubusercontent.com/77573775/188975108-29369900-bab7-4210-af07-b1fd48729fdf.png">
</p>

#### Visualising internal properties of beam2

I prefer using shader nodes to visualise internal properties of all deformable elements. The most simple one is `beam2` element. `beam2` have only one evaluation point in its central. Therefore, I design shader node for `beam2` same as the image bellow. For each internal property value, I turn them 
into corresponding color in `RGB` shader node to set the same color for the whole `beam2` object.
<p align = "center">
<img width="500" alt="beam2 shader node" src="https://user-images.githubusercontent.com/77573775/188975188-c9289018-3b00-4481-94b3-799710065c4c.png">
</p>

#### Visualising internal properties of beam3

`beam3` element has two evaluation points in the middle of the first and second parts. They have their own internal properties values in each time point. 
Therefore, design `beam3` shader node as bellow using `Texture Coordinate` and `ColorRamp` with linear interpolation method to show the change of internal properties along `beam3` element. In addition, I use middle node object (`_RF2`) for source of coordinate in `Texture Coordinate` shader node.

<p align = "center">
<img width="500" alt="beam3 shader node" src="https://user-images.githubusercontent.com/77573775/188975207-6ed02176-084a-4954-9303-6fbc775e2789.png">
</p>

#### Visualising internal properties of shell4 and membrane4

`shell4` and `membrane4` elements have upto 4 evaluation points in middle of their edges. Therefore, I have to use 2 `ColorRamp` shader nodes with linear interpolation. In addition, I also create one hidden empty object named `elem.name` + `_ctr` which have average location and rotation of the visualising element. The object is used as source of coordinate in `Texture Coordinate` shader node.

<p align = "center">
<img width="500" alt="shell4 and membrane4 shader node" src="https://user-images.githubusercontent.com/77573775/188975243-b2084bd0-39f3-46bf-8d74-c90a9144f6f4.png">
</p>

Currently, internal force and internal moment outputs for `shell4` and `membrane4` elements haven't implemented in MBDyn yet. I could only make some manual demo for the visualisation as the image bellow.

<p align = "center">
<img width="500" alt="shell4 result" src="https://user-images.githubusercontent.com/77573775/188975268-9ba67b7a-540e-4626-895f-e8f3b42fa4ff.png">
</p>

## Example
- [Beam Slider](https://drive.google.com/file/d/171uiprQyw_J72Nsp5sEqcG8kWWpCnhNx/view?usp=sharing)
- [ssbeam](https://drive.google.com/file/d/1TXppjv6WpJqlQhvARd366sICb9ht08Bg/view?usp=sharing)
- [modalbody](https://drive.google.com/file/d/1uNdeA9CxwyOstFpUzMQZ2g-rxqbT490W/view?usp=sharing)
- [Internal Force](https://drive.google.com/file/d/1_Nkq-vMmqwKmg-e0klxUiFTCTKL3caoF/view?usp=sharing)

## Usage
#### Plotting with `matplotlib`
* Step1: Load MBDyn output file (`.nc` file)
* Step2: Select Scene Properties> MBDyn Data Plot
* Step3: Choose plotting variale (For example: `node.struct.1.X`)
* Step4: Choose plotting engine (Matplotlib), ploting dimension, plot frequency and plot type
* Step5: Click on `Plot variable`
* Step6: Open `Image Editer` Window and open the latest image to see the graph results

[Guidance Video](https://drive.google.com/file/d/12Wgo8uCcu9zVAzMgaOK-MAZGgz6bz_WM/view?usp=sharing)

#### Plotting with `bokeh`
* Step1: Load MBDyn output file (`.nc` file)
* Step2: Select `Scene Properties` > `MBDyn Data Plot`
* Step3: Choose plotting variale (For example: `node.struct.1.X`)
* Step4: Choose plotting engine (Bokeh), ploting dimension, plot frequency, plot type and output format (`show in localhost` or `save as png`)
* Step5: Click on `Plot variable`
If you choose `show in localhost`, one localhost would appear on your browser with the graph
* (If you choose `save as png`) Step6: Open `Image Editer` window and open the latest image to see the graph results

[Guidance Video](https://drive.google.com/file/d/1G7EQGE_luLYgjK5RKH96COUPUuKI0mca/view?usp=sharing)

#### Visualising `modal` as `Blendyn Components`
* Step1: Load MBDyn output file (Make sure to have `.mod` file in the same directory)
* Step2: `Standard Import`
* Step3: Select `Scene Properties` > `MBDyn Components` > `Add new component`
* Step4: Choose elements for the component (start with "modal_")
* Step5: Choose `.fem` file for this modal
* Step6: `Load fem file` >  `Import all modal nodes` > `Add element`
Repeat step 4,5,6 if there is more than one modal
* Step7: Choose `Mesh Object`
* Step8: `Confirm`

[Guidance Video 1](https://drive.google.com/file/d/18WK2ro4a3yRr5K2rym4f9KG6K-qlem2Z/view?usp=sharing)

[Guidance Video 2](https://drive.google.com/file/d/1PnPyJq08O2StzeUhV4gJyDGnYKEsoTjD/view?usp=sharing)

#### Visualising internal force or internal moment for deformable elements
* Step1: Load MBDyn output file (`.nc` file)
* Step2: Tick on `Import internal property`
* Step3: Choose `Type` (internal property type), `Dim` (dimension), `Min` (Lower boundary), `Max` (Upper boundary)
* Step4: `Standard Import`
* Step5: `Animate Scene`
* Step6: Choose `Material Preview` in `Viewport Shading`

[Guidance Video](https://drive.google.com/file/d/1sXWVIqQN-BjtGM46LsZ8YKfdhACUZJYE/view?usp=sharing)

## My strace
- [Blendyn development proposal](https://docs.google.com/document/d/13YRZmQG28vJxjMJzE7wCPlxZB1kvi0N0aN4e53q4rfM/edit?usp=sharing)
- [Weekly reports](https://public.gitlab.polimi.it/DAER/mbdyn/-/wikis/GSoC-Students-Blogs)

## Future work
- Finishing internal force and internal moment visualisation for `shell4` and `membrane4` elements when their MBDyn outputs are available.
- Eventhough I have successed in visualising internal force and internal moment for `beam2` and `beam3`, choosing correct value for `Min` and `Max` boundary is challenge. `Autosetup Boundary` can't solve that problem effectively. I hope that, we can have a better way to implement it in the future.
- Making 3D ploting for 'trajectory' plot type and show it in a `localhost` when ploting with `bokeh`
