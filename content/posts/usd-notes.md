+++
title = "USD Notes"
date = 2022-01-26
draft = false

[taxonomies]
categories = ["Programming", "CG"]
tags = ["usd", "programming", "cg"]
+++

Here's some notes I have on creating USD scenes from Maya. This'll hopefully grow and possibly become a bit more coherent.

## Resources

- <https://github.com/ColinKennedy/USD-Cookbook>

## Merging the USD cache output with the stages

```python
import mayaUsd
import pxr

cache_stage = pxr.Usd.Stage.Open("/path/to/someenv.usd")
layer_stage = mayaUsd.lib.GetPrim("stageShape1").GetStage()

attach_point = cache_stage.GetPrimAtPath("/someEnv/usd/stage1")
layer_point = layer_stage.GetPrimAtPath("/assets")

attach_point.GetPath().pathString
pxr.Sdf.CopySpec(layer_stage.GetRootLayer(), layer_point.GetPath(), cache_stage.GetRootLayer(), attach_point.GetPath())
print(pxr.Sdf.CopySpec(layer_stage.GetRootLayer(), layer_point.GetPath(), cache_stage.GetRootLayer(), attach_point.GetPath()))

cache_stage.GetRootLayer().Export("/path/to/someenv_merged.usd")
```

## Saving a layer from Maya USD

```python
import mayaUsd

stage = mayaUsd.lib.GetPrim("UsdStageShape1").GetStage()
root_layer = stage.GetRootLayer()

stage.Export("/path/to/stage.usd")  # Exports flattened
root_layer.Export("/path/to/layer.usd")  # Exports diff
```

## Querying all of the nodes and attrs

```python
from __future__ import print_function

import pxr

stage = pxr.Usd.Stage.Open("/path/to/my.usd")

prim = stage.GetPrimAtPath("/")

stack = [prim]

while stack:
    prim = stack.pop()

    print(prim)

    for attr in prim.GetAttributes():
        print('\t', attr, type(attr.Get()), attr.Get())

    stack.extend(prim.GetAllChildren())
```

Example output:

```
Usd.Prim(</geo/head_grp/eyes_grp/eye_rt_hi>)
	 Usd.Prim(</geo/head_grp/eyes_grp/eye_rt_hi>).GetAttribute('purpose') <type 'str'> default
	 Usd.Prim(</geo/head_grp/eyes_grp/eye_rt_hi>).GetAttribute('visibility') <type 'str'> inherited
	 Usd.Prim(</geo/head_grp/eyes_grp/eye_rt_hi>).GetAttribute('xformOp:rotateXYZ') <class 'pxr.Gf.Vec3f'> (0.45326722, -9.807732, 0.008041994)
	 Usd.Prim(</geo/head_grp/eyes_grp/eye_rt_hi>).GetAttribute('xformOp:scale') <class 'pxr.Gf.Vec3f'> (1.128, 1.128, 1.128)
	 Usd.Prim(</geo/head_grp/eyes_grp/eye_rt_hi>).GetAttribute('xformOp:translate') <class 'pxr.Gf.Vec3d'> (-3.3591980934143066, 178.04139340134995, 7.005110197231562)
	 Usd.Prim(</geo/head_grp/eyes_grp/eye_rt_hi>).GetAttribute('xformOpOrder') <class 'pxr.Vt.TokenArray'> [xformOp:translate, xformOp:rotateXYZ, xformOp:scale]
```
