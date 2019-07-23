![Design Patterns For Humans](./cover.png)

<p align="center">
🎉 超简单的设计模式指南！ 🎉
</p>
<p align="center">
设计模式这个话题往往让人感到困惑。在这里我会尝试以<i>最简单<i>的方式来解释它，并且尽量能够让你们熟记于心（也许是我自己）。
<br/>
本文基于 <a href="https://github.com/kamranahmedse/design-patterns-for-humans">"Design patterns for humans"</a>
</p>
  
🚀 介绍
=================

设计模式是**如何处理某些重复问题**的指南和解决方案。它们并不是类、包或者库等能够集成到你的应用程序中去，更不会在你的应用中产生任何魔法。相反，设计模式是如何在特定情景下解决特定问题的通用指南。

维基百科是这么解释设计模式的：

> 在[软件工程](https://zh.wikipedia.org/wiki/軟體工程)中，设计模式是对[软件设计](https://zh.wikipedia.org/wiki/軟件設計)中普遍存在（反复出现）的各种问题，所提出的解决方案。设计模式并不直接用来完成[代码](https://zh.wikipedia.org/wiki/%E7%A8%8B%E5%BC%8F%E7%A2%BC)的编写，而是描述在各种不同情况下，要怎么解决问题的一种方案。

⚠️ 注意
-----------------
- 设计模式并不是解决问题的银弹。
- 不要刻意地去使用设计模式，否则可能会带来坏的结果。要记住设计模式是问题的解决方案，而不是发现问题的解决方案，因此不要滥用它们。
- 如果在正确的地方以正确的方式使用设计模式，它们就会有所帮助。若不是这样，则会让你的代码变得一团糟。


## 🐢 在开始之前

- 所有的设计模式都是基于 JavaScript 中最新的 ES6 语法实现。
- 因为 JavaScript 并没有接口的实现，所以本文中的例子使用的都是隐喻的接口，这意味着，只要一个类具有特定接口应该拥有的属性和方法，那就认为它实现了这个接口。为了更容易地告诉我们正在使用的接口，你可以在每个例子的注释中找到相关的信息。

Types of Design Patterns
-----------------

* [Creational](#creational-design-patterns)
* [Structural](#structural-design-patterns)
* [Behavioral](#behavioral-design-patterns)


Creational Design Patterns
==========================

In plain words
> Creational patterns are focused towards how to instantiate an object or group of related objects.

Wikipedia says
> In software engineering, creational design patterns are design patterns that deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. The basic form of object creation could result in design problems or added complexity to the design. Creational design patterns solve this problem by somehow controlling this object creation.

 * [Simple Factory](#-simple-factory)
 * [Factory Method](#-factory-method)
 * [Abstract Factory](#-abstract-factory)
 * [Builder](#-builder)
 * [Prototype](#-prototype)
 * [Singleton](#-singleton)

🏠 Simple Factory
--------------
Real world example
> Consider, you are building a house and you need doors. It would be a mess if every time you need a door, you put on your carpenter clothes and start making a door in your house. Instead you get it made from a factory.

In plain words
> Simple factory simply generates an instance for client without exposing any instantiation logic to the client

Wikipedia says
> In object-oriented programming (OOP), a factory is an object for creating other objects – formally a factory is a function or method that returns objects of a varying prototype or class from some method call, which is assumed to be "new".

**Programmatic Example**

First of all we have a door interface and the implementation
```js
/*
Door

getWidth()
getHeight()

*/

class WoodenDoor {
  constructor(width, height){
    this.width = width
    this.height = height
  }

  getWidth(){
    return this.width
  }

  getHeight(){
    return this.height
  }
}
```
Then we have our door factory that makes the door and returns it

```js
const DoorFactory = {
  makeDoor : (width, height) => new WoodenDoor(width, height)
}
```
And then it can be used as

```js
const door = DoorFactory.makeDoor(100, 200)
console.log('Width:', door.getWidth())
console.log('Height:', door.getHeight())
```

**When to Use?**

When creating an object is not just a few assignments and involves some logic, it makes sense to put it in a dedicated factory instead of repeating the same code everywhere. 

🏭 Factory Method
--------------

Real world example
> Consider the case of a hiring manager. It is impossible for one person to interview for each of the positions. Based on the job opening, she has to decide and delegate the interview steps to different people. 

In plain words
> It provides a way to delegate the instantiation logic to child classes. 

Wikipedia says
> In class-based programming, the factory method pattern is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created. This is done by creating objects by calling a factory method—either specified in an interface and implemented by child classes, or implemented in a base class and optionally overridden by derived classes—rather than by calling a constructor.

 **Programmatic Example**

Taking our hiring manager example above. First of all we have an interviewer interface and some implementations for it

```js
/*
Interviewer interface

askQuestions()
*/

class Developer {
  askQuestions() {
    console.log('Asking about design patterns!')
  }
}

class CommunityExecutive {
  askQuestions() {
    console.log('Asking about community building')
  }
}
```

Now let us create our `HiringManager`

```js
class HiringManager {
        
    takeInterview() {
        const interviewer = this.makeInterviewer()
        interviewer.askQuestions()
    }
}
```
Now any child can extend it and provide the required interviewer
```js
class DevelopmentManager extends HiringManager {
    makeInterviewer() {
        return new Developer()
    }
}

class MarketingManager extends HiringManager {
    makeInterviewer() {
        return new CommunityExecutive()
    }
}
```
and then it can be used as

```js
const devManager = new DevelopmentManager()
devManager.takeInterview() // Output: Asking about design patterns

const marketingManager = new MarketingManager()
marketingManager.takeInterview() // Output: Asking about community buildng.
```

**When to use?**

Useful when there is some generic processing in a class but the required sub-class is dynamically decided at runtime. Or putting it in other words, when the client doesn't know what exact sub-class it might need.

🔨 Abstract Factory
----------------

Real world example
> Extending our door example from Simple Factory. Based on your needs you might get a wooden door from a wooden door shop, iron door from an iron shop or a PVC door from the relevant shop. Plus you might need a guy with different kind of specialities to fit the door, for example a carpenter for wooden door, welder for iron door etc. As you can see there is a dependency between the doors now, wooden door needs carpenter, iron door needs a welder etc.

In plain words
> A factory of factories a factory that groups the individual but related/dependent factories together without specifying their concrete classes. 

Wikipedia says
> The abstract factory pattern provides a way to encapsulate a group of individual factories that have a common theme without specifying their concrete classes

**Programmatic Example**

Translating the door example above. First of all we have our `Door` interface and some implementation for it

```js
/*
Door interface :

getDescription()
*/

class WoodenDoor {
    getDescription() {
        console.log('I am a wooden door')
    }
}

class IronDoor {
    getDescription() {
        console.log('I am an iron door')
    }
}
```
Then we have some fitting experts for each door type

```js
/*
DoorFittingExpert interface :

getDescription()
*/

class Welder {
    getDescription() {
        console.log('I can only fit iron doors')
    }
}

class Carpenter {
    getDescription() {
        console.log('I can only fit wooden doors')
    }
}
```

Now we have our abstract factory that would let us make family of related objects i.e. wooden door factory would create a wooden door and wooden door fitting expert and iron door factory would create an iron door and iron door fitting expert
```js
/*
DoorFactory interface :

makeDoor()
makeFittingExpert()
*/

// Wooden factory to return carpenter and wooden door
class WoodenDoorFactory {
    makeDoor(){
        return new WoodenDoor()
    }

    makeFittingExpert() {
        return new Carpenter()
    }
}

// Iron door factory to get iron door and the relevant fitting expert
class IronDoorFactory {
    makeDoor(){
        return new IronDoor()
    }

    makeFittingExpert() {
        return new Welder()
    }
}
```
And then it can be used as
```js
woodenFactory = new WoodenDoorFactory()

door = woodenFactory.makeDoor()
expert = woodenFactory.makeFittingExpert()

door.getDescription()  // Output: I am a wooden door
expert.getDescription() // Output: I can only fit wooden doors

// Same for Iron Factory
ironFactory = new IronDoorFactory()

door = ironFactory.makeDoor()
expert = ironFactory.makeFittingExpert()

door.getDescription()  // Output: I am an iron door
expert.getDescription() // Output: I can only fit iron doors
```

As you can see the wooden door factory has encapsulated the `carpenter` and the `wooden door` also iron door factory has encapsulated the `iron door` and `welder`. And thus it had helped us make sure that for each of the created door, we do not get a wrong fitting expert.   

**When to use?**

When there are interrelated dependencies with not-that-simple creation logic involved

👷 Builder
--------------------------------------------
Real world example
> Imagine you are at Hardee's and you order a specific deal, lets say, "Big Hardee" and they hand it over to you without *any questions* this is the example of simple factory. But there are cases when the creation logic might involve more steps. For example you want a customized Subway deal, you have several options in how your burger is made e.g what bread do you want? what types of sauces would you like? What cheese would you want? etc. In such cases builder pattern comes to the rescue.

In plain words
> Allows you to create different flavors of an object while avoiding constructor pollution. Useful when there could be several flavors of an object. Or when there are a lot of steps involved in creation of an object.

Wikipedia says
> The builder pattern is an object creation software design pattern with the intentions of finding a solution to the telescoping constructor anti-pattern.

Having said that let me add a bit about what telescoping constructor anti-pattern is. At one point or the other we have all seen a constructor like below:

```js
constructor(size, cheese = true, pepperoni = true, tomato = false, lettuce = true) {
    // ... 
}
```

As you can see the number of constructor parameters can quickly get out of hand and it might become difficult to understand the arrangement of parameters. Plus this parameter list could keep on growing if you would want to add more options in future. This is called telescoping constructor anti-pattern.

**Programmatic Example**

The sane alternative is to use the builder pattern. First of all we have our burger that we want to make

```js
class Burger {
    constructor(builder) {
        this.size = builder.size
        this.cheeze = builder.cheeze || false
        this.pepperoni = builder.pepperoni || false
        this.lettuce = builder.lettuce || false
        this.tomato = builder.tomato || false
    }
}
```

And then we have the builder

```js
class BurgerBuilder {

    constructor(size) {
        this.size = size
    }

    addPepperoni() {
        this.pepperoni = true
        return this
    }

    addLettuce() {
        this.lettuce = true
        return this
    }

    addCheeze() {
        this.cheeze = true
        return this
    }

    addTomato() {
        this.tomato = true
        return this
    }

    build() {
        return new Burger(this)
    }
}
```
And then it can be used as:

```js
const burger = (new BurgerBuilder(14))
    .addPepperoni()
    .addLettuce()
    .addTomato()
    .build()
```

__Javascript specific tip__ : When you find that the number of arguments to a function or method are too many (normally any more than 2 arguments is considered too much), use a single object argument instead of multiple arguments. This serves two purposes :

1. It makes your code look less cluttered, since there is only one argument.
2. You don't have to worry about the order of arguments 
