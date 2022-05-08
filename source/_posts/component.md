---
title: Component
date: 2022-05-08 13:55:10
tags:
    - Unity
    - Unity Basic
    - Component
categories:
    - Program
    - Game Development
    - Unity
description: Basic knowledge of Component in Unity
top_img: /img/component.png
cover: /img/component-cover.png
---

# Introduce
Unity components are functions that you add to GameObject for a specific purpose

# Table of contents
* What is component?
* Basic ways to use component
    + Add
    + Delete
    + Get
    + Switch On/Off
* Other techniques
* Summary

# What is component?
A component is a function that you add to a GameObject.
Specifically, it is as follows :
```
Camera : Camera component
Light : Light component
Collider detection : Collider component
Image display : SpriteRenderer
Mesh display : MeshRenderer
Button : Button component
```
![image](https://user-images.githubusercontent.com/44673303/167288871-a5a6f894-296f-49da-a197-9aa1af173291.png)

All the functions that we usually call "camera" and "light" are component
You can add as many components as you want to a GameObject.

Unity adds component to the GameObject by adding them. You can freely create the function of the game.

# Feature of compoent
## Component can be added to GameObject

    The biggest feature of the component is that it can be added to the GameObject.

    On the flip side, a component can't do anything by itself.
    It only works when you add it to the GameObject.

## Component require inherit MonoBehavior class to attach to GameObject

    Components can also be created by the developer himself.
    Character control, game logic, etc.
    This is the so-called game function development.

    You can add your own components to GameObject by inheriting the MonoBehaviour class.

## Multiple same components can be added

    You can add many of the same components to a GameObject.
![image](https://user-images.githubusercontent.com/44673303/167289965-bd0463a7-8c16-4145-840a-fc33fa329bdc.png)
    
    The same components are independent of each other.
    Looking at the image below we can see that their `Instance ID` is different
![image](https://user-images.githubusercontent.com/44673303/167290053-20a1e913-8a66-474e-be1d-fc0c91c811aa.png)

# Basic ways to use component

## Add component

The first to remember is how to add a component to a GameObject.
There are actually four ways to add a component

###  Add Component button in the Inspector window

![image](https://user-images.githubusercontent.com/44673303/167290605-c2900eaa-d20e-4b15-8e7d-379bace6629c.png)

### Add from menu Component

you need select GameObject want add component first
![image](https://user-images.githubusercontent.com/44673303/167290740-13a370e6-ffb3-47d7-a212-87f5dae55015.png)

### Drag and drop from the Project window

you need select GameObject want add component first

![image](https://user-images.githubusercontent.com/44673303/167290844-b7554d62-0113-4341-9283-fb75929800c3.png)

Note: you need to hold without just clicking on the component file script otherwise the Inspector window will show the component file's information
You can also use the lock inspector feature to lock the current inspector display for easy drag and drop
![lock_inspector](https://user-images.githubusercontent.com/44673303/167291252-9478bcb7-3494-4d73-ab6b-636c5ea9bf47.gif)

Or using feature multiple inspector in Unity 2021+ to open another inspector for Assets specified and it is separate from the main inspector.

You can open it by right-clicking on any asset or GameObject in your project and selecting Properties at the bottom of the menu that appears 
or by left-clicking it using the keyboard shortcut Alt + P
![image](https://user-images.githubusercontent.com/44673303/167291422-d48b42fc-89b8-4d68-b665-8afe67dcef61.png)

### Add Component with script

The methods mentioned earlier are all manipulations with Editor.
The next way will be manipulated by script. You can use the method AddComponent

```csharp
public Component AddComponent(System.Type componentType){..};
public T AddComponent<T>() where T : Component => this.AddComponent(typeof (T)) as T;
```

For example add PlayerMovement component to GameObject
```csharp
public class Player : MonoBehaviour
{
    void Start()
    {
        gameObject.AddComponent(typeof(PlayerMovement));
        gameObject.AddComponent<PlayerMovement>();
    }
}
```
AddComponent is just a function of GameObject to execute AddComponent for GameObject
Remember this because it's a method you use a lot.

## Delete component

There are 2 ways to remove component from GameObject

### Remove component menu in inspector

![image](https://user-images.githubusercontent.com/44673303/167292192-6ee26d6f-c4cc-4e0c-b341-70d09ed99999.png)

### Use method Destroy form script

```csharp
/// <summary>
///   <para>Removes a GameObject, component or asset.</para>
/// </summary>
/// <param name="obj">The object to destroy.</param>
/// <param name="t">The optional amount of time to delay before destroying the object.</param>
[ExcludeFromDocs]
public static void Destroy(Object obj)
{
    float t = 0.0f;
    Object.Destroy(obj, t);
}

/// <summary>
///   <para>Removes a GameObject, component or asset.</para>
/// </summary>
/// <param name="obj">The object to destroy.</param>
/// <param name="t">The optional amount of time to delay before destroying the object.</param>
[NativeMethod(IsFreeFunction = true, Name = "Scripting::DestroyObjectFromScripting", ThrowsException = true)]
[MethodImpl(MethodImplOptions.InternalCall)]
public static extern void Destroy(Object obj, [DefaultValue("0.0F")] float t);
```
The following sample code is an example of adding a new PlayerMovement and deleting it after 1 second.
The process of waiting for 1 second uses a function called coroutine.

```csharp
using System.Collections;
using UnityEngine;

public class Player : MonoBehaviour
{
    private IEnumerator Start()
    {
        var playerMovement = gameObject.AddComponent(typeof(PlayerMovement));
        //or you can use var playerMovement = gameObject.AddComponent<PlayerMovement>();

        yield return new WaitForSeconds(1f);
        Destroy(playerMovement);
    }
}
```
or can be written in another way as follows
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private void Start()
    {
        var playerMovement = gameObject.AddComponent(typeof(PlayerMovement));
        //or you can use var playerMovement = gameObject.AddComponent<PlayerMovement>();

        Destroy(playerMovement, 1);
    }
}
```

PlayerMovement was added and removed after 1 second.

Use Destroy alot so be sure to remember it

## Get Component

There are three ways to get components.

### Use GetComponent

The first is the introduction of the GetComponent method.

Gets the component attached to a single GameObject.

Source code of GetComponent
```csharp
[SecuritySafeCritical]
public unsafe T GetComponent<T>()
{
    CastHelper<T> castHelper = new CastHelper<T>();
    this.GetComponentFastPath(typeof (T), new IntPtr((void*) &castHelper.onePointerFurtherThanT));
    return castHelper.t;
}

/// <summary>
///   <para>Returns the component of Type type if the game object has one attached, null if it doesn't.</para>
/// </summary>
/// <param name="type">The type of Component to retrieve.</param>
[TypeInferenceRule(TypeInferenceRules.TypeReferencedByFirstArgument)]
[FreeFunction(HasExplicitThis = true, Name = "GameObjectBindings::GetComponentFromType", ThrowsException = true)]
[MethodImpl(MethodImplOptions.InternalCall)]
public extern Component GetComponent(System.Type type);
```
It can be obtained by specifying the component name.

Let's look at the specific code.
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private void Awake()
    {
        // get the component (PlayerMovenent)
        var playerMovement = GetComponent<PlayerMovement>();
    }
}
```
I'm getting a PlayerMovement using the GetComponent method.

![image](https://user-images.githubusercontent.com/44673303/167293387-0bb7ee59-4c5a-4ad5-9ddd-05633c6838d1.png)

It can be obtained PlayerMovement component when PlayerMovement is attached to the same GameObject as AddPlayerComponent as shown above.

**If you cannot get the component by GetComponet**

The component may not be pre-attached to the GameObject.
In that case, the return value of GetComponent is null.

If it's null, you may want to attach a new component.
You can solve it with the following sample.
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private void Awake()
    {
        // get the component (PlayerMovenent)
        var playerMovement = GetComponent<PlayerMovement>();
        // if it does not exist, attach a new one
        if (playerMovement != null)
        {
            playerMovement = gameObject.AddComponent<PlayerMovement>();
        }
    }
} 
```
If BoxCollider does not exist, it is newly addedComponent.

It's a technique you use a lot, so keep it in mind.

**GetComponents to get all component of Type in the GameObject**

As explained in "Component Features", it is possible to attach the same component multiple times.
![image](https://user-images.githubusercontent.com/44673303/167293762-ac234a8e-c643-4fcd-b345-a5281e706045.png)

To get all these same components at once use **GetComponents**

Similar to GetComponent, but note that it has an "s" at the end.
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private void Awake()
    {
        PlayerMovement[] playerMovements = GetComponents<PlayerMovement>();
    }
}
```
Results return an array, you can get each component individually by querying this returned array

### Use GetComponentInChildren

GameObject can have a hierarchy as shown in the figure below.

![hierarchy](https://user-images.githubusercontent.com/44673303/167303116-9d90c9bb-d075-491d-846d-c069d95b5344.gif)

GetComponent introduced earlier can only get one level of component
You can also get the components of the child hierarchy by using GetComponentInChildren, which will be introduced below

Like the hierarchical tree image above the Movement object is located last

We will use GetComponentInChildren to get the PlayerMovement component attached to the Movement object from the Player component attached to the Player object.
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private void Awake()
    {
        PlayerMovement playerMovement = GetComponentInChildren<PlayerMovement>();
    }
} 
```

**- Notes: GetComponentInChildren is also include check for yourself**

As the name suggests, GetComponentInChildren tends to target only the child hierarchy, but in fact, it also targets itself.

![image](https://user-images.githubusercontent.com/44673303/167304191-a1420b27-b430-4f82-99a8-e5aefd31eda2.png)

Note that the search range for GetComponentInChildren is a child hierarchy that includes itself, as shown above.

**- How to exclude inactive components from search**

I used GetComonentInChildren to get the components of the child hierarchy.
In some cases, you may want to exclude inactive components.

In other words, it looks like the figure below.

![image](https://user-images.githubusercontent.com/44673303/167304526-e7c26d8d-40fc-48a1-a55e-d80503e8f353.png)

This means that inactive GameObject components are excluded from the search.

It's easy to do.
Assign true to the argument of GetComponentInChildren.
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private void Awake()
    {
        // Assign true to the argument to exclude inactive components
        PlayerMovement playerMovement = GetComponentInChildren<PlayerMovement>(true);
    }
} 
```
Then PlayerMovement of the inactive GameObject will be excluded from the search target.

There is one caveat. Only "inactive GameObjects" are excluded from the search.
If only the component is inactive, it will not be excluded from the search.

![image](https://user-images.githubusercontent.com/44673303/167304818-9df2fbb6-5842-48ed-aee2-c70a13244f4d.png)

Please note that if the component is disable as shown above, it will not be excluded from the search target.

**- Get Multiple Components in Child Hierarchy with GetComponentsInChildren**

Like GetComponent, GetComponentInChildren provides a way to get more than one component of Type.
That is GetComponentsInChildren. Like GetComponent, it has an "s".

For example:
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private void Awake()
    {
        PlayerMovement[] playerMovement = GetComponentsInChildren<PlayerMovement>();
    }
} 
```
```csharp
// Assign true to the argument to exclude inactive components
PlayerMovement[] playerMovement = GetComponentsInChildren<PlayerMovement>(true);
```

GetComponentsInChildren also excludes inactive components from the acquisition target if the argument is set to true.

### GetComponentInParent

You can search the parent hierarchy by using GetComponentInParent.
For example:
```csharp
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    private void Start()
    {
        var player = GetComponentInParent<Player>();
    }
}
```
Get the component of the parent hierarchy from yourself as shown in the figure below.

![image](https://user-images.githubusercontent.com/44673303/167305392-6ceecfda-ff52-4c95-86cf-04b7c9d1c699.png)

GetComponentInParent, like any other method

You can get multiple components by using "GetComponentsInParent".

### On/Off component with enable


![image](https://user-images.githubusercontent.com/44673303/167305653-0f65dadc-f054-46f8-8488-51342fe33b32.png)
![image](https://user-images.githubusercontent.com/44673303/167305681-7cd1c383-cdbd-4fe7-a1e7-2106655ddc22.png)

What is this ON / OFF affecting?
Specific event functions such as Start and Update.

If unchecked, these event functions will not be executed.

If you do not write a specific event function, the checkbox will not be displayed.

There are a total of 7 specific event functions.
1. Start()
2. Update()
3. FixedUpdate()
4. LateUpdate()
5. OnGUI()
6. OnDisable()
7. OnEnable()

Checkboxes appear when you write these functions.

As mentioned above, checkboxes are controlled by the enabled property.

The range controlled by this enabled property is the seven event functions introduced earlier.

If you uncheck it, it will not be executed automatically from Unity.

# Other techniques
Now that you know the basic usage of components. Here are 6 more techniques to use.

## Specify the component to be attached with RequireComponent
It's convenient to use RequestComponent for components that you always want to include

There are some components that are always added as "component attached component" when you add Components.
For example, the Image component.

![image_component](https://user-images.githubusercontent.com/44673303/167306302-b23667d9-3f06-45e3-9dee-562ef69f0110.gif)

As you can see in the video above, the CanvasRenderer component is added at the same time as the Image component is added.

The functionality added as "component attached component" can be easily realized by the developer himself using his RequireComponent.
The procedure is simple.

Just write [RequireComponent (typeof (component name))] in the class definition part.

For example:
```csharp
using UnityEngine;

[RequireComponent(typeof(PlayerData))]
public class Player : MonoBehaviour
{
}
```

![require_component](https://user-images.githubusercontent.com/44673303/167306737-d24ca5e0-f3f2-4280-bfcf-f1e99ec34491.gif)

Then if you add the Player Component to the GameObject like this, the PlayerData will automatically be added

**The component specified by RequireComponent cannot be deleted**

![remove_require_component](https://user-images.githubusercontent.com/44673303/167307038-55414e63-cc9f-405e-8e87-687938053946.gif)

If you want to delete the component specified by RequireComponent, you can handle it in the following two ways.

     Stop RequireComponent
     Delete RequireComponent from the component that describes it

It is recommended to use RequireComponent in this way because you can avoid forgetting to attach the component.

This is a component technique that you should definitely remember.

Like the example above, you need to remove the Player component first before you can remove the PlayerData component

## Disallow Multiple Component prohibits duplication of components

There are cases where you do not want to attach the same component to one GameObject more than once.

This can be achieved by using DisallowMultipleComponent.

```csharp
using UnityEngine;

[DisallowMultipleComponent]
public class Player : MonoBehaviour
{
}
```

![disable_multiple](https://user-images.githubusercontent.com/44673303/167307677-19a74a57-4625-4cff-ae35-5628dcc86249.gif)

_DisallowMultipleComponent also prohibits addition from scripts_

At first glance, DisallowMultipleComponent seems to be a limitation only on the Unity editor, but it actually blocks AddComponent from scripts as well.

While running Unity, you will get the following log of duplicate AddComponent attempts and no component is added either.

```csharp
Can't add 'Player' to Player because a 'Player' is already added to the game object!
UnityEngine.GameObject:AddComponent<Player> ()
```

Keep in mind that DisallowMultipleComponent is useful when you absolutely want to set only one component.

## Add Component Menu

You can actually add your own components to this "Component Menu"

It's very easy to do.
Just add an AddComponentMenu before the component's class.
```csharp
using UnityEngine;

[AddComponentMenu("Gamespace/Unit/Player")]
public class Player : MonoBehaviour
{
}
```
![image](https://user-images.githubusercontent.com/44673303/167307985-f280e35c-8768-44e6-9c8f-92f4021d0307.png)

AddComponentMenu helps you organize and group components for ease of use

## TryGetComponent

I introduced using GetComponent to get a component.

The specified component may not exist at the time you need get, isn't it?

So you can use TryGetComponent. TryGetComponent is a method that returns true if it succeeds in getting the component and false if it fails.

```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private void Start()
    {
        if (TryGetComponent<PlayerMovement>(out var movement))
        {
            // PlayerMovement has been attached and you can get it
        }
        else
        {
            // Couldn't get PlayerMovement (it didn't attached in the first place)
        }
    }
}
```

Of course, the same effect can be obtained by the null check of the acquired component mentioned above.
You can use TryGetComponent (Since Unity 2019.2) to check if an object has a component, it will not allocate GC in the **EDITOR** if the object doesn't have one.
Please choose the one you like.

Whichever you choose, there is no impact on performance.

## Add / Delete components to multiple GameObjects at once

Have you ever wanted to add a component to multiple GameObjects at once?

Actually it is possible. Select all GameObject you want to add in the Hierarchy window and add the component.
Then you can add them all at once and shorten the work time.

![add_multiple](https://user-images.githubusercontent.com/44673303/167308833-599fecbf-c6db-4950-8552-a7cca28c5075.gif)

The same applies to deletion.

If you are adding the same component. Just select multiple GameObjects and click Remove Component.

It's not a technique you use often, but it's a useful technique that can save you time if you know it.

![remove_multiple](https://user-images.githubusercontent.com/44673303/167308834-f9316552-f9c1-499b-bcd1-238e9aa99cc2.gif)

## Component Reference

Let see example below
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private PlayerMovement _movement;

    private void Awake()
    {
        _movement = GetComponentInChildren<PlayerMovement>();
    }
}
```

will be changed to
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    [SerializeField] private PlayerMovement movement;
}
```

or 
```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    public PlayerMovement movement;
}
```

You couldn't get the PlayerMovement just by replacing it.
You need to drag and drop on the following Inspector window.

![serialized](https://user-images.githubusercontent.com/44673303/167309149-df58ef3f-e882-4dd5-8eda-6c450c50722d.gif)

By declaring a reference variable  the amount of code is reduced and the GetComponent processing load at the time of runtime is also eliminated.

Of course, this technique is limited to cases where you can set a reference in advance.
It cannot be used when you want to declare dynamically components in the game.

To preserve the encapsulation of OOP . Please use [SerializeField]

## Search the entire scene FindObjectOfType

In addition to the GetComponent method, there is also a method called FindObjectOfType that searches the entire scene.

Methods introduced earlier was limited to relative GameObjects such as itself, parent hierarchy, and child hierarchy.

```csharp
using UnityEngine;

public class Player : MonoBehaviour
{
    private void Start()
    {
        // Returns the first active loaded object of Type type in scene
        var movement = FindObjectOfType<PlayerMovement>();
    }
}
```
FindObjectOfType is a useful method for getting a component that is somewhere in the scene.

Use FindObjectsOfType if the scene has multiple components

```csharp
PlayerMovement[] movement = FindObjectsOfType<PlayerMovement>();
```
You can get all the PlayerMovement in the scene as an array.

Both FindObjectsOfType and FindObjectsOfType can exclude inactive components from the search target. This is the same as GetComponentInChildren.
````csharp
PlayerMovement[] movement = FindObjectsOfType<PlayerMovement>(true);
````

# Notes

## Component new is prohibited

Do not use new when scripting a component.
Be sure to use AddComponent.

If you are using new, you will see a warning in the Console window similar to the following:
![image](https://user-images.githubusercontent.com/44673303/167309842-37c744de-7411-4a5f-b3e6-03a0951f7902.png)
```
You are trying to create a MonoBehaviour using the 'new' keyword.
This is not allowed. MonoBehaviours can only be added using AddComponent().
Alternatively, your script can inherit from ScriptableObject or no base class at all
```

## GetComponentInChildren and GetComponentInParent get the first one found

GetComponentInChildren searches the child hierarchy and GetComponentInParent searches the parent hierarchy. 
These methods get the first one found.

The important thing is the order of searching.

Both search from nearby GameObjects starting from themselves.

![image](https://user-images.githubusercontent.com/44673303/167310085-df80e217-00a0-46f2-84c8-218eed7191df.png)
![image](https://user-images.githubusercontent.com/44673303/167310229-d4c59231-5a62-4380-bc29-c406a4984b70.png)

Be careful when multiple identical components exist.

## Do not use FindObjectOfType as much as possible
In Unity Document
```
Object.FindObjectOfType will not return Assets (meshes, textures, prefabs, ...) or inactive objects.
It will not return an object that has HideFlags.DontSave set.

Please note that this function is very slow. It is not recommended to use this function every frame.
In most cases you can use the singleton pattern instead.
```

It is better not to use FindObjectOfType as much as possible.

The reason is that he has a heavy load of searching the entire scene.
If there are a lot of GameObjects in the scene, his clumsiness in processing will be noticeable.
In other words, it does not move smoothly and makes the user uncomfortable.

Therefore, let's find out where to use FindObjectOfType.
It's a good time to initialize the game so that it doesn't matter if the game gets stuck

FindObjectOfType is useful. While convenient, it also has a large risk.
As explained above, it is important to determine the usage.

In addition to FindObjectOfType, use of Find method should be careful about performance.

## The access modifier of the event function is invalid.

The access modifier of the event function can be anything.
Access modifiers are keywords such as publich and private that you give to classes, methods, and variables.

These specify the access range for classes, methods and variables.
Normally, the access modifier is set in consideration of the intention of use, but
Ignored for event functions.

It will be executed whether it is public or private.
All you have to do is write an event function. But for me I like to fully declare and clear the access modifier so that everything is consistent

Even if the component is disabled (enabled is false), it is possible to make the event function a public method and execute it from the outside.
```csharp
// In case of public method, it can be called from the outside
public void Start ()
{
}
```

As shown above, the Start method is made public.
Keep in mind that turning a component off doesn't mean it won't be called.

# Summary
