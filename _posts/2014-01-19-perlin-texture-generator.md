---
layout: post
title: Perlin texture generator
---

Over the last weekend I spent my time writing a procedural Javascript texture generator with [@bwanner](https://github.com/bwanner) using the [perlin noise method](https://en.wikipedia.org/wiki/Perlin_noise). It generates monochrome textures based on a lattice of random values. By defining lattice distances and interpolating (bilinear/bicubic) between the generated random values, one can produce effects like marble or cloudy structures.

You can **[try the texture generator](http://schickling.me/algorithms/#/perlin)** and checkout the [source code](https://github.com/schickling/algorithms/blob/master/app/scripts/services/graphics/perlin.coffee).

![](/img/perlin.png)
