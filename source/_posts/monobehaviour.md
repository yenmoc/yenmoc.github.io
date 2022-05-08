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
* What is Unity's MonoBehaviour?
	+ MonoBehaviour is used for objects that appear on the screen
* Component inherit form MonoBehaviour
	+ What is component?
	+ MonoBehaviour itself is also a component
	+ What is inheritance of MonoBehavior
* Methods that are automatically called by MonoBehaviour inheritance
	+ Awake
	+ Start
	+ Update
	+ OnEnable
	+ OnDisable
	+ OnDestroy
	+ ...
	+ Notes on the event function
* Frequently used MonoBehaviour functions
	+ Coroutine: Suspend and resume processing
	+ enabled: On/Off MonoBehaviour
* Cases that do not use MonoBehaviour
	+ You do not need to use GameObject
	+ That uses only c#
	+ Recommended to use MonoBehaviour
* Common problems with MonoBehaviour
	+ Script cannot be attached to GameObject
	+ Event functions such as Awake and Start are not called
	+ MonoBehaviour 'new' is prohibited
* Summary


# What is Unity's MonoBehaviour
MonoBehaviour is a base class for programming in Unity

You can add many 'functions' to GameObject through attaching the MonoBehaviour component to the Gameobject

**1. When is MonoBehaviour used?**
 - One of theme is the object that appears on the screen

For example, you may want to make character appear in the game and move it

	=> MonoBehaviour is used for that character
![image](https://user-images.githubusercontent.com/44673303/167283647-0b691a5b-7466-4361-ba17-dcdbcf2ab9b3.png)

As you see Playe_Male use component PlayerManager (Inherit MonoBehaviour)

![image](https://user-images.githubusercontent.com/44673303/167283797-15dd46fd-747b-4c71-b27a-8d68d8878494.png)

UI objects (user interface) also use MonoBehaviour

In other words, MonoBehaviour is used for the objects that appear on the screen.

# Component inherit from MonoBehaviour
A concept "component" that cannot be removed when programing with unity.
Understanding the components and MonoBehaviour is essential

In conclusion, when a developer creates his own function (component).
He must inherit MonoBehaviour

**So what is component?**
The "components" used in Unity are the "features" of the game you develop
Strictly speaking, it is a function added to GameObject.

The component does not work by itself. Only by adding it to the GameObject will the component work
For example, 