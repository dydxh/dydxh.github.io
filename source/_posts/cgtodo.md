---
title: CG TODO
date: 2018-11-10 19:50:18
tags: CG
---

马上就 CG proposal 了，大概列一下目前已经能做的和已知可以做的技术。两个月的时间肯定不可能全部做完，因为还没有开始糊框架，按照喜好自己挑着往上加吧。
<!-- more -->

参考资料目前也只看过[《GPU Gems》](https://developer.nvidia.com/gpugems)和 [LearnOpenGL](https://learnopengl-cn.github.io/) 还有一些零零碎碎的网站，以及 [ShaderToy](https://www.shadertoy.com/)。

当务之急是快速的糊出来框架然后把已经实现的效果迅速的做出来，其他的特效之后再去学，因为游戏的可扩展空间非常高。

## TODO
- [x] Shader
  
- [x] Texture
    - [x] 2D texture loader
    - [x] Skybox loader

- [ ] Camera
    - [x] Eular angle
    - [ ] Quaternion

- [ ] Model
    - [x] Obj loader
    - [ ] Animation obj loader

- [ ] Light
    - [x] Direct lighting
    - [x] Point lighting
    - [x] Gamma correction
    - [ ] Light Volume
    - [x] HDR
    - [x] Bloom
    - [x] Gauss Blur
    - [ ] Motion Blur
    - [x] SSAO

- [x] Anti-aliasing MSAA

- [x] Shadow
    - [x] Shadow map
    - [x] PCF

- [ ] Water Wave
    - [x] Gerstner
    - [ ] FFT

- [ ] Object Picking

- [x] Particle system

- [ ] Font display

- [ ] Simple physics system
    - [ ] Gravity
    - [x] Collide AABB
    - [ ] Speed
    - [ ] Acceleration

- [ ] LOD - Level of Details

- [ ] SSR - Screen Space Reflection

- [ ] Infinite lawn

- [ ] Interference - Bubble render
