title: Unity学习笔记
date: 2016-04-18 11:46:55
tags:
---

## Render Mode(渲染模式)
* Screen space - Overlay
> 在场景中UI被渲染在屏幕上(即使在场景中没有相机，也将呈现UI)。如果屏幕大小改变或更改了分辨率，画布将自动更改大小，以很好的适配屏幕。

* Screen space - Camera
> 类似于Screen Space - Overlay。但在这种渲染模式下，画布被放置在指定相机前的一个给定的距离上，通过指定的相机UI被呈现出来，Camera setting会影响到UI的现实。如果屏幕大小改变或更改了分辨率，画布将自动更改大小，以很好的适配屏幕。
* World Space
> 在这种模式下呈现UI，UI好像是3D场景中的一个平面plane对象。与上面两种不同，plane不需要面对镜头，可以是面向你喜欢的任意方向。可以使用Rect Transform设置画布的大小，但其屏幕的大小将取决于拍摄的角度和相机的距离。场景中的其他对象可以pass behind，通过或者在画布上面。

## Canvas Scaler
* Constant Pixel Size
> 以像素进行缩放
* Scale With Screen Size
> 以宽度为标准进行缩放
* Constant Physical Size
> 以物理尺寸进行缩放