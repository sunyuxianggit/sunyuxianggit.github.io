---
title: Blender脚本学习笔记
date: 2020-03-10 19:14:04
tags: blender
---


### 编辑器

- 安装python3.0以上(Blender 2.8是 python3.x)

- Vscode 安装python 和 blender Development 插件

- 终端安装 fake-bpy 包``pip install fake-bpy-module-2.80``

   https://github.com/nutti/fake-bpy-module

  安装完上面这些，vscode里面就会有自动补全

  Debug

- ctrl + shift + P, select "Blender: Start"

- 选择你的blender安装路径, vscode会连接到blender

- ctrl + shift + P, "Blender: Run script" 并且可以使用断点调试

   <!-- more -->


### 插件路径

- 目录：user\AppData\Roaming\Blender Foundation\Blender\scripts\addons,代表非系统原生的用户插件,blender所有安装的外部插件都会被解压放置到这个文件夹下。

  安装插件可以在blender的addon界面直接选择zip文件安装，也可以把插件文件夹直接拖入此目录。

- 目录：D:\blender-2.81a\2.81\scripts\addons

  第二个是软件原生插件路径，不建议将自己写的插件放入此地，此地不少系统插件的代码可在以后做参考用，值得了解。

### 如何查看命令 

- 类似maya 直接执行某个功能，看info输出命令（A+X	清空nfo）
- 选中命令直接 Copy Data Path.
- 鼠标悬停在某个命令上看提示，如果没有提示，要去perfence里面的interface下勾选Python Tooltips.
- 控制台直接输入``dir（某个模块名）``查看输出
- 打开blender的text editor，很多模板文件可供使用：
- 用python console中的.后代码提示快捷键自动补全查看方法（ctrl+space）
- 查询api
- 全球最大爱好者论坛https://blender.stackexchange.com/
- 查看各类开源插件

### 基础知识

- bpy 意思是 blender python

- 常规得到某个物体信息流程 

  1 viewport选中物体 

  2 查看Transform的提示 

  3 调用各种信息 类似 .location

常用命令

``bpy.context.`` 正文，当前环境的所有内容的意思

``bpy.context.object`` 物体

``mesh = bpy.data.objects["mesh"]`` Mesh变量存储物体mesh

``bpy.ops.object.select_all(action='SELECT')``全选命令

``bpy.ops.object.select_all(action='DESELECT')``全不选命令

``bpy.context.view_layer.objects.active = mesh``大纲选中，激活物体

``bpy.context.object.location`` 拿到选中物体的世界坐标

``bpy.context.object.location.z``拿到选中物体的世界坐标中的Z坐标数值

``bpy.context.object.location.z += random()`` 选中物体的世界坐标中的Z坐标数值随机加一个数

``bpy.context.object.name``查看选中物体的名字

``bpy.ops.object.duplicate(linked=False,mode='TRANSLATION')``复制模型

``bpy.ops.object.modifier_add(type='DECIMATE')``添加编辑器

``bpy.context.object.modifiers["Decimate"].ratio = 0.1``设置编辑器参数

``bpy.ops.object.modifier_apply(apply_as='DATA', modifier="Decimate")``#应用编辑器

``bpy.context.object.name = "要改的名字"`` 选中物体的名字

``bpy.ops.mesh.primitive_cube_add()``创建box

``bpy.context.object.active_material``返回当前选择物体的材质球

``bpy.data.materials.get(材质球名称字符串)``拿到当前场景的某个材质

``bpy.context.view_layer.objects.active.material_slots.data.active_material= 某个材质`` 给当前激活的物体材质球插槽赋予某个材质

``bpy.ops.wm.save_mainfile(filepath="E:\\Test\\testsave.blend") ``存储当前文件

``bpy.ops.wm.open_mainfile(filepath=BlendFilePath)`` 打开文件

得到当前版本的blender文件夹路径

```
import sys
argv = sys.argv #当前blender的路径例：['D:\\blender-2.81a\\blender.exe']
```

遍历列表选中物体：

```
for i in bpy.context.visible_objects:#迭代所有可见物体
	if i.name == "要选物体的名字":
		i.select_set(state=True)
for i in bpy.context.visible_objects:
    if i.type == "MESH":#判断物体类型是模型
        bpy.context.view_layer.objects.active = i #当前激活物体定义为i
        bpy.ops.object.mode_set(mode='EDIT') #编辑模式
        bpy.context.tool_settings.mesh_select_mode = (False, True, False)#编辑模式的（点线面）
        bpy.ops.mesh.select_all(action='SELECT')#全选命令
        bpy.ops.object.mode_set(mode='OBJECT')#关闭编辑模式
```

