# Final Report of MBDyn GSoC-2022 project: Blendyn Development
by Do Tien Dung, September, 2022

## Project overview and objective
[Blendyn](https://github.com/zanoni-mbdyn/blendyn) is one of post-processing tools of [MBDyn](https://www.mbdyn.org/) - multibody simulation engine. Blendyn is built as an Add-ons of [Blender](https://www.blender.org/) which allow user to visualise MBDyn output datas with 3D animations. Blendyn was first implemented in 2017. However, there are some rooms for improvement and some parts of current Blendyn was out of date. In this project, I focus on developing some of them:
1. Improving graph plotting function and adding two new plotting engine into Blendyn `matplotlib` and `bokeh` beside `pygal`
2. Visualising two MBDyn elements: `beam slider` and `modal`
3. Visualising internal force and internal moment for deformable elements: `beam2`, `beam3`, `shell4`, `membrane4`
4. Improving UI

## Code
- [Blendyn Github branch "devel2"](https://github.com/zanoni-mbdyn/blendyn/tree/devel2) 
- [File Change](https://github.com/zanoni-mbdyn/blendyn/pull/51/files)
- [Commits History](https://github.com/zanoni-mbdyn/blendyn/pull/51/commits)

## Example
- [Beam Slider]()
- [ssbeam]()
- [modalbody]()
- [Internal Force]()

## Usage Video
### Plotting with `matplotlib`

### Plotting with `bokeh`

### Visualising `modal` as `Blendyn Components`

### Visualising internal force or internal moment for deformable elements


## My strace for GSoC 2022
- [Blendyn development proposal](https://docs.google.com/document/d/13YRZmQG28vJxjMJzE7wCPlxZB1kvi0N0aN4e53q4rfM/edit?usp=sharing)
- [Weekly reports](https://public.gitlab.polimi.it/DAER/mbdyn/-/wikis/Google-Summer-of-Code/Students-Blogs)

## Future improvement
- Finishing internal force and internal moment visualisation for `shell4` and `membrane4` elements when their MBDyn output avaialble.
- Making 3D ploting for 'trajectory' plot type and show it in a `local host` when ploting with `bokeh`
