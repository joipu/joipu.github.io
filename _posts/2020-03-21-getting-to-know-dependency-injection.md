---
layout: single
title:  "Getting to know Dependency Injection"
date:   2020-03-21 15:21:56 -0700
categories: 
    - Development Notes
tags:
    - dependency injection
    - design
    - java
classes: wide
---

Last Edited: March 21, 2020 8:21 PM

Lately I joined the recommendation infra team and started to write more code in Java. While getting familar with the codebase, I found the concept of the inversion of control and dependency injection very intriguing, and I hoped to write down my understanding about the topic in this post. 


## What is Dependency Injection?
Dependency injection is a common technique used for making class independent of dependencies, which in other words is trying to keep the coupling between classes as 'low' as possible. Why does it matter to keep the degree of coupling low and make classes less dependent on each other? This is the idea behind the dependency inversion principle (DIP). We can see an example below.

Suppose if we want to design a house, we may want to create a Door class, a Skeleton class, and a House class. Conventionally, we could briefly write these classes with their relationships as below:

Door class:
```java
public class Door {
	private int height;

	public Door() {
		this.height = 7;
	}
}
```	

Skeleton class:
```java
public class Skeleton {
	private Door door;

	public Skeleton() {
		this.door = new Door();
	}
}
```	

House class:
```java
public class House {
	private Skeleton skeleton;

	public House() {
		this.skeleton = new Skeleton();
	}

	public void build() {
		...
	}
}
```	

With these classes, we can create a house by simply calling:
```java
House house = new House();
house.build();
```

The House class has a hard dependency on the Skeleton class which has a hard dependency on the Door class. By saying A class has a hard dependency on B class, it means the module A cannot function without B. It looks fine though at this moment, until if we want to add more variables or simply change the status to the class which other classes depend. For example, if we want to make the height in Door class changeable, we may change the code to this:

Door class:
```java
public class Door {
	private int height;

	public Door(int height) {
		this.height = height;
	}
}
```	

Accordingly, the other classes should also be changed:

Skeleton class:
```java
public class Skeleton {
	private Door door;

	public Skeleton(int height) {
		this.door = new Door(height);
	}
}
```	

House class:
```java
public class House {
	private Skeleton skeleton;

	public House(int height) {
		this.skeleton = new Skeleton(height);
	}

	public void build() {
		...
	}
}
```	

We can see that the change in the Door class leads to a whole bunch of necessary changes in other dependendable classes, which would make the code difficult to maintain if this type of cases happens in a large codebase. What we could potentially do to avoid the issue, is instead of doing this traditional control flow allowing top classes to be dependent on lower classes, we invert the control flow to do backwards. See the change in example below:

First create a Door class and add the instance vairable height:
```java
public class Door {
	private int height;

	public Door (int height) {
		this.height = height;
	}
}
```

Then we create the Skeleton class and the House class as following:
```java
public class Skeleton {
	private Door door;

	// Dependency Injection
	public Skeleton(Door door) {
		this.door = door;
	}
}

```

```java
public class House {
	private Skeleton skeleton;

	// Dependency Injection
	public House(Skeleton skeleton) {
		this.skeleton = skeleton;
	}
}
```

With the code changed as above, we can create our house by calling:
```java
Door door = new Door(8); // suppose the door has a height of 8 feet
Skeleton skeleton = new Skeleton(door);
House house = new House(skeleton);

house.build();

```

This version allows us to change the height to be whatever we want without affecting other dependent classes, like Skeleton or House. The way we pass the skeleton into the constructor in House is called Dependency Injection, specifically constructor injection. Instead of initializing the dependency in the class contructor (e.g. public House() { this.skeleton = new Skeleton(); }), we inject the dependency (e.g. public House(Skeleton skeleton) { this.skeleton = skeleton; }. This way we can avoid changing the signature of dependendable classes when a class's signature gets changed as the dependendable class's contructor (House) does not directly depend on the other class (Skeleton). It follows the design pattern Inversion of Control and removes the hard dependencies between classes, which makes the code more maintainable.


## What is IoC Container?
The IoC container is the framework that is used to achieve automatic dependency injection. From the updated House example above, we can easily see that in order to build a house, we actually need to do multiple create to instantiate the relevant objects. This could lead to a lot of duplicate work which could be resolved by the IoC Container. The container automatically initialize all these relevant objects and wrap them up. With its help, we don't have to manually initialize these objects or try to go over all the relevant contructors and see what's happening under the hood when we want to create a House object. The IoC container helps us do that. 

Examples of the IoC container include Google Guice, Spring IoC, and StructureMap. There are many resources and documentations online that introduce how to use these tools. I may or may not write another post to further talk about the IoC container or any of these tools, or maybe add more thoughts about inversion of controler/dependency injection in general. But as for now, I will stop here.
