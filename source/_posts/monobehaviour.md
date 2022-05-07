---
title: MonoBehaviour
date: 2022-05-07 11:54:04
tags:
	- Unity
	- Unity Basic
	- Component
categories:
	- Program
	- Game Development
	- Unity
description: Basic knowledge of monobehaviour
top_img: /img/mono.png
cover: /img/mono-cover.png
---

# Introduce
The foundation is important for everything.
MonoBehaviour is the foundation of Unity.
This is an indispensable function for developing games.
You will definitely create the functionality of the game you develop with MonoBehaviour

So let's learn about MonoBehaviour.

# Table of contents
- What is Unity's MonoBehaviour?
	+ MonoBehaviour is used for objects that appear on the screen
- Component inherit form MonoBehaviour
	+ What is component?
	+ MonoBehaviour itself is also a component
	+ What is inheritance of MonoBehavior
- Methods that are automatically called by MonoBehaviour inheritance
	+ Awake
	+ Start
	+ Update
	+ OnEnable
	+ OnDisable
	+ OnDestroy
	+ ...
	+ Notes on the event function
- Frequently used MonoBehaviour functions
	+ Coroutine: Suspend and resume processing
	+ enabled: On/Off MonoBehaviour
- Cases that do not use MonoBehaviour
	+ You do not need to use GameObject
	+ That uses only c#
	+ Recommended to use MonoBehaviour
- Common problems with MonoBehaviour
	+ Script cannot be attached to GameObject
	+ Event functions such as Awake and Start are not called
	+ MonoBehaviour 'new' is prohibited
- Summary


# What is Unity's MonoBehaviour
MonoBehaviour is a base class for programmung in Unity

Take a look at the following Monobehaviour usage example
```c#
```