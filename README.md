![Design Patterns For Humans](./cover.png)

<p align="center">
🎉 超简单的设计模式指南！ 🎉
</p>
<p align="center">
设计模式这个话题往往令人感到困惑。在这里我会尝试以最简单的方式来解释它，并且尽量能够让读者熟记于心（也许是我自己）。
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

- 本文所有的设计模式都是基于 JavaScript 中最新的 ES6 语法实现。
- 因为 JavaScript 并没有接口的实现，所以本文中的例子使用的都是隐喻的接口，这意味着，只要一个类具有特定接口应该拥有的属性和方法，那就认为它实现了这个接口。为了更容易地告诉我们正在使用的接口，你可以在每个例子的注释中找到相关的信息。

设计模式类型
-----------------

* [创建型](#creational-design-patterns)
* [结构型](#structural-design-patterns)
* [行为型](#behavioral-design-patterns) 


创建型设计模式
==========================

简单地来说

> 创建型模式专注于如何实例化一个对象或者一组相关的对象。

维基百科上的解释

> 在软件工程中，创建型模式是处理对象创建的设计模式，试图根据实际情况使用合适的方式创建对象。基本的对象创建方式可能会导致设计上的问题，或增加设计的复杂度。创建型模式通过以某种方式控制对象的创建来解决问题。

 * [简单工厂模式](#-simple-factory)
 * [工厂方法模式](#-factory-method)
 * [抽象工厂模式](#-abstract-factory)
 * [建立者模式](#-builder)
 * [原型模式](#-prototype)
 * [单例模式](#-singleton)

🏠 简单工厂模式
--------------
现实生活中的例子

> 考虑，你正在建造一座房子并且你需要一些门。如果每次当你需要一扇门的时候，你就得穿上木匠的衣服，开始在你的房子里做一扇门，那将会变得一团糟。相反，你可以从工厂中得到它。

简单地来说

> 简单工厂模式只是生成一个实例，而不会暴露任何实例化的逻辑给客户端。

维基百科上的解释

> 在面向对象编程 (OOP) 中，工厂是一个用来创建其他对象的对象 - 从形式上来讲工厂是一个函数或者方法，调用它能够返回不同原型或类的对象，而这些对象往往通过 **new** 生成。

**编程示例**

首先我们会创建一个 door 接口并实现这个接口

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
然后我们会创建一个能够制造门并且返回它的 door 工厂

```js
const DoorFactory = {
  makeDoor : (width, height) => new WoodenDoor(width, height)
}
```
最后它可以这样使用

```js
const door = DoorFactory.makeDoor(100, 200)
console.log('Width:', door.getWidth())
console.log('Height:', door.getHeight())
```

**什么时候使用？**

当创建一个对象不仅仅是一些赋值和一些逻辑时，将其放在一个专用的工厂而不是到处重复相同的代码，这样的做法往往是有意义的。

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
2. You don't have to worry about the order of arguments since arguments are now passed as named properties of the object.

For example : 

```js
const burger = new Burger({
    size : 14,
    pepperoni : true,
    cheeze : false,
    lettuce : true,
    tomato : true
})
```

instead of :

```
const burger = new Burger(14, true, false, true, true)
```

**When to use?**

When there could be several flavors of an object and to avoid the constructor telescoping. The key difference from the factory pattern is that factory pattern is to be used when the creation is a one step process while builder pattern is to be used when the creation is a multi step process.


💍 Singleton
------------
Real world example
> There can only be one president of a country at a time. The same president has to be brought to action, whenever duty calls. President here is singleton.

In plain words
> Ensures that only one object of a particular class is ever created.

Wikipedia says
> In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system.

Singleton pattern is actually considered an anti-pattern and overuse of it should be avoided. It is not necessarily bad and could have some valid use-cases but should be used with caution because it introduces a global state in your application and change to it in one place could affect in the other areas and it could become pretty difficult to debug. The other bad thing about them is it makes your code tightly coupled plus it mocking the singleton could be difficult.

**Programmatic Example**

In javascript, singletons can be implemented using the module pattern. Private variables and functions are hidden in a function closure, and public methods are selectively exposed.
```js
const president = (function(){
    const presidentsPrivateInformation = 'Super private'

    const name = 'Turd Sandwich'

    const getName = () => name

    return {
        getName
    }
}())
```

Here, `presidentsPrivateInformation` and `name` are kept private. However, `name` can be accessed with the exposed `president.getName` method.

```js
president.getName() // Outputs 'Turd Sandwich'
president.name // Outputs undefined
president.presidentsPrivateInformation // Outputs undefined
```

Structural Design Patterns
==========================
In plain words
> Structural patterns are mostly concerned with object composition or in other words how the entities can use each other. Or yet another explanation would be, they help in answering "How to build a software component?"

Wikipedia says
> In software engineering, structural design patterns are design patterns that ease the design by identifying a simple way to realize relationships between entities.

 * [Adapter](#-adapter)
 * [Bridge](#-bridge)
 * [Composite](#-composite)
 * [Decorator](#-decorator)
 * [Facade](#-facade)
 * [Flyweight](#-flyweight)
 * [Proxy](#-proxy)

🔌 Adapter
-------
Real world example
> Consider that you have some pictures in your memory card and you need to transfer them to your computer. In order to transfer them you need some kind of adapter that is compatible with your computer ports so that you can attach memory card to your computer. In this case card reader is an adapter.
> Another example would be the famous power adapter a three legged plug can't be connected to a two pronged outlet, it needs to use a power adapter that makes it compatible with the two pronged outlet.
> Yet another example would be a translator translating words spoken by one person to another

In plain words
> Adapter pattern lets you wrap an otherwise incompatible object in an adapter to make it compatible with another class.

Wikipedia says
> In software engineering, the adapter pattern is a software design pattern that allows the interface of an existing class to be used as another interface. It is often used to make existing classes work with others without modifying their source code.

**Programmatic Example**

Consider a game where there is a hunter and he hunts lions.

First we have an interface `Lion` that all types of lions have to implement

```js
/*
Lion interface :

roar()
*/

class AfricanLion  {
    roar() {}
}

class AsianLion  {
    roar() {}
}
```
And hunter expects any implementation of `Lion` interface to hunt.
```js
class Hunter {
    hunt(lion) {
        // ... some code before
        lion.roar()
        //... some code after
    }
}
```

Now let's say we have to add a `WildDog` in our game so that hunter can hunt that also. But we can't do that directly because dog has a different interface. To make it compatible for our hunter, we will have to create an adapter that is compatible

```js
// This needs to be added to the game
class WildDog {
    bark() {
    }
}

// Adapter around wild dog to make it compatible with our game
class WildDogAdapter {

    constructor(dog) {
        this.dog = dog;
    }
    
    roar() {
        this.dog.bark();
    }
}
```
And now the `WildDog` can be used in our game using `WildDogAdapter`.

```js
wildDog = new WildDog()
wildDogAdapter = new WildDogAdapter(wildDog)

hunter = new Hunter()
hunter.hunt(wildDogAdapter)
```

🚡 Bridge
------
Real world example
> Consider you have a website with different pages and you are supposed to allow the user to change the theme. What would you do? Create multiple copies of each of the pages for each of the themes or would you just create separate theme and load them based on the user's preferences? Bridge pattern allows you to do the second i.e.

![With and without the bridge pattern](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

In Plain Words
> Bridge pattern is about preferring composition over inheritance. Implementation details are pushed from a hierarchy to another object with a separate hierarchy.

Wikipedia says
> The bridge pattern is a design pattern used in software engineering that is meant to "decouple an abstraction from its implementation so that the two can vary independently"

**Programmatic Example**

Translating our WebPage example from above. Here we have the `WebPage` hierarchy

```js
/*
Webpage interface :

constructor(theme)
getContent()
*/

class About{ 
    constructor(theme) {
        this.theme = theme
    }
    
    getContent() {
        return "About page in " + this.theme.getColor()
    }
}

class Careers{
   constructor(theme) {
       this.theme = theme
   }
   
   getContent() {
       return "Careers page in " + this.theme.getColor()
   } 
}
```
And the separate theme hierarchy
```js
/*
Theme interface :

getColor()
*/

class DarkTheme{
    getColor() {
        return 'Dark Black'
    }
}
class LightTheme{
    getColor() {
        return 'Off white'
    }
}
class AquaTheme{
    getColor() {
        return 'Light blue'
    }
}
```
And both the hierarchies
```js
const darkTheme = new DarkTheme()

const about = new About(darkTheme)
const careers = new Careers(darkTheme)

console.log(about.getContent() )// "About page in Dark Black"
console.log(careers.getContent() )// "Careers page in Dark Black"
```

🌿 Composite
-----------------

Real world example
> Every organization is composed of employees. Each of the employees has same features i.e. has a salary, has some responsibilities, may or may not report to someone, may or may not have some subordinates etc.

In plain words
> Composite pattern lets clients to treat the individual objects in a uniform manner.

Wikipedia says
> In software engineering, the composite pattern is a partitioning design pattern. The composite pattern describes that a group of objects is to be treated in the same way as a single instance of an object. The intent of a composite is to "compose" objects into tree structures to represent part-whole hierarchies. Implementing the composite pattern lets clients treat individual objects and compositions uniformly.

**Programmatic Example**

Taking our employees example from above. Here we have different employee types

```js
/*
Employee interface :

constructor(name, salary)
getName()
setSalary()
getSalary()
getRoles()
*/

class Developer {

    constructor(name, salary) {
        this.name = name
        this.salary = salary
    }

    getName() {
        return this.name
    }

    setSalary(salary) {
        this.salary = salary
    }

    getSalary() {
        return this.salary
    }

    getRoles() {
        return this.roles
    }

    develop() {
        /* */
    }
}

class Designer {

    constructor(name, salary) {
        this.name = name
        this.salary = salary
    }

    getName() {
        return this.name
    }

    setSalary(salary) {
        this.salary = salary
    }

    getSalary() {
        return this.salary
    }

    getRoles() {
        return this.roles
    }

    design() {
        /* */
    }
}
```

Then we have an organization which consists of several different types of employees

```js
class Organization {
    constructor(){
        this.employees = []
    }

    addEmployee(employee) {
        this.employees.push(employee)
    }

    getNetSalaries() {
        let netSalary = 0

        this.employees.forEach(employee => {
            netSalary += employee.getSalary()
        })

        return netSalary
    }
}
```

And then it can be used as

```js
// Prepare the employees
const john = new Developer('John Doe', 12000)
const jane = new Designer('Jane', 10000)

// Add them to organization
const organization = new Organization()
organization.addEmployee(john)
organization.addEmployee(jane)

console.log("Net salaries: " , organization.getNetSalaries()) // Net Salaries: 22000
```

☕ Decorator
-------------

Real world example

> Imagine you run a car service shop offering multiple services. Now how do you calculate the bill to be charged? You pick one service and dynamically keep adding to it the prices for the provided services till you get the final cost. Here each type of service is a decorator.

In plain words
> Decorator pattern lets you dynamically change the behavior of an object at run time by wrapping them in an object of a decorator class.

Wikipedia says
> In object-oriented programming, the decorator pattern is a design pattern that allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class. The decorator pattern is often useful for adhering to the Single Responsibility Principle, as it allows functionality to be divided between classes with unique areas of concern.

**Programmatic Example**

Lets take coffee for example. First of all we have a simple coffee implementing the coffee interface

```js
/*
Coffee interface:
getCost()
getDescription()
*/

class SimpleCoffee{

    getCost() {
        return 10
    }

    getDescription() {
        return 'Simple coffee'
    }
}
```
We want to make the code extensible to allow options to modify it if required. Lets make some add-ons (decorators)
```js
class MilkCoffee {


    constructor(coffee) {
        this.coffee = coffee
    }

    getCost() {
        return this.coffee.getCost() + 2
    }

    getDescription() {
        return this.coffee.getDescription() + ', milk'
    }
}

class WhipCoffee {

    constructor(coffee) {
        this.coffee = coffee
    }

    getCost() {
        return this.coffee.getCost() + 5
    }

    getDescription() {
        return this.coffee.getDescription() + ', whip'
    }
}

class VanillaCoffee {

    constructor(coffee) {
        this.coffee = coffee
    }

    getCost() {
        return this.coffee.getCost() + 3
    }

    getDescription() {
        return this.coffee.getDescription() + ', vanilla'
    }
}

```

Lets make a coffee now

```js
let someCoffee

someCoffee = new SimpleCoffee()
console.log(someCoffee.getCost())// 10
console.log(someCoffee.getDescription())// Simple Coffee

someCoffee = new MilkCoffee(someCoffee)
console.log(someCoffee.getCost())// 12
console.log(someCoffee.getDescription())// Simple Coffee, milk

someCoffee = new WhipCoffee(someCoffee)
console.log(someCoffee.getCost())// 17
console.log(someCoffee.getDescription())// Simple Coffee, milk, whip

someCoffee = new VanillaCoffee(someCoffee)
console.log(someCoffee.getCost())// 20
console.log(someCoffee.getDescription())// Simple Coffee, milk, whip, vanilla
```

📦 Facade
----------------

Real world example
> How do you turn on the computer? "Hit the power button" you say! That is what you believe because you are using a simple interface that computer provides on the outside, internally it has to do a lot of stuff to make it happen. This simple interface to the complex subsystem is a facade.

In plain words
> Facade pattern provides a simplified interface to a complex subsystem.

Wikipedia says
> A facade is an object that provides a simplified interface to a larger body of code, such as a class library.

**Programmatic Example**
Taking our computer example from above. Here we have the computer class

```js
class Computer {

    getElectricShock() {
        console.log('Ouch!')
    }

    makeSound() {
        console.log('Beep beep!')
    }

    showLoadingScreen() {
        console.log('Loading..')
    }

    bam() {
        console.log('Ready to be used!')
    }

    closeEverything() {
        console.log('Bup bup bup buzzzz!')
    }

    sooth() {
        console.log('Zzzzz')
    }

    pullCurrent() {
        console.log('Haaah!')
    }
}
```
Here we have the facade
```js
class ComputerFacade
{
    constructor(computer) {
        this.computer = computer
    }

    turnOn() {
        this.computer.getElectricShock()
        this.computer.makeSound()
        this.computer.showLoadingScreen()
        this.computer.bam()
    }

    turnOff() {
        this.computer.closeEverything()
        this.computer.pullCurrent()
        this.computer.sooth()
    }
}
```
Now to use the facade
```js
const computer = new ComputerFacade(new Computer())
computer.turnOn() // Ouch! Beep beep! Loading.. Ready to be used!
computer.turnOff() // Bup bup buzzz! Haah! Zzzzz
```

🍃 Flyweight
---------

Real world example
> Did you ever have fresh tea from some stall? They often make more than one cup that you demanded and save the rest for any other customer so to save the resources e.g. gas etc. Flyweight pattern is all about that i.e. sharing.

In plain words
> It is used to minimize memory usage or computational expenses by sharing as much as possible with similar objects.

Wikipedia says
> In computer programming, flyweight is a software design pattern. A flyweight is an object that minimizes memory use by sharing as much data as possible with other similar objects it is a way to use objects in large numbers when a simple repeated representation would use an unacceptable amount of memory.

**Programmatic example**
Translating our tea example from above. First of all we have tea types and tea maker

```js
// Anything that will be cached is flyweight. 
// Types of tea here will be flyweights.
class KarakTea {
}

// Acts as a factory and saves the tea
class TeaMaker {
    constructor(){
        this.availableTea = {}
    }

    make(preference) {
        this.availableTea[preference] = this.availableTea[preference] || (new KarakTea())
        return this.availableTea[preference]
    }
}
```

Then we have the `TeaShop` which takes orders and serves them

```js
class TeaShop {
    constructor(teaMaker) {
        this.teaMaker = teaMaker
        this.orders = []
    }

    takeOrder(teaType, table) {
        this.orders[table] = this.teaMaker.make(teaType)
    }

    serve() {
        this.orders.forEach((order, index) => {
            console.log('Serving tea to table#' + index)
        })
    }
}
```
And it can be used as below

```js
const teaMaker = new TeaMaker()
const shop = new TeaShop(teaMaker)

shop.takeOrder('less sugar', 1)
shop.takeOrder('more milk', 2)
shop.takeOrder('without sugar', 5)

shop.serve()
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

🎱 Proxy
-------------------
Real world example
> Have you ever used an access card to go through a door? There are multiple options to open that door i.e. it can be opened either using access card or by pressing a button that bypasses the security. The door's main functionality is to open but there is a proxy added on top of it to add some functionality. Let me better explain it using the code example below.

In plain words
> Using the proxy pattern, a class represents the functionality of another class.

Wikipedia says
> A proxy, in its most general form, is a class functioning as an interface to something else. A proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. Use of the proxy can simply be forwarding to the real object, or can provide additional logic. In the proxy extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked.

**Programmatic Example**
Taking our security door example from above. Firstly we have the door interface and an implementation of door

```js
/*
Door interface :

open()
close()
*/

class LabDoor {
    open() {
        console.log('Opening lab door')
    }

    close() {
        console.log('Closing the lab door')
    }
}
```
Then we have a proxy to secure any doors that we want
```js
class Security {
    constructor(door) {
        this.door = door
    }

    open(password) {
        if (this.authenticate(password)) {
            this.door.open()
        } else {
        	console.log('Big no! It ain\'t possible.')
        }
    }

    authenticate(password) {
        return password === 'ecr@t'
    }

    close() {
        this.door.close()
    }
}
```
And here is how it can be used
```js
const door = new Security(new LabDoor())
door.open('invalid') // Big no! It ain't possible.

door.open('ecr@t') // Opening lab door
door.close() // Closing lab door
```

Behavioral Design Patterns
==========================

In plain words
> It is concerned with assignment of responsibilities between the objects. What makes them different from structural patterns is they don't just specify the structure but also outline the patterns for message passing/communication between them. Or in other words, they assist in answering "How to run a behavior in software component?"

Wikipedia says
> In software engineering, behavioral design patterns are design patterns that identify common communication patterns between objects and realize these patterns. By doing so, these patterns increase flexibility in carrying out this communication.

* [Chain of Responsibility](#-chain-of-responsibility)
* [Command](#-command)
* [Iterator](#-iterator)
* [Mediator](#-mediator)
* [Memento](#-memento)
* [Observer](#-observer)
* [Visitor](#-visitor)
* [Strategy](#-strategy)
* [State](#-state)
* [Template Method](#-template-method)

🔗 Chain of Responsibility
-----------------------

Real world example
> For example, you have three payment methods (`A`, `B` and `C`) setup in your account each having a different amount in it. `A` has 100 USD, `B` has 300 USD and `C` having 1000 USD and the preference for payments is chosen as `A` then `B` then `C`. You try to purchase something that is worth 210 USD. Using Chain of Responsibility, first of all account `A` will be checked if it can make the purchase, if yes purchase will be made and the chain will be broken. If not, request will move forward to account `B` checking for amount if yes chain will be broken otherwise the request will keep forwarding till it finds the suitable handler. Here `A`, `B` and `C` are links of the chain and the whole phenomenon is Chain of Responsibility.

In plain words
> It helps building a chain of objects. Request enters from one end and keeps going from object to object till it finds the suitable handler.

Wikipedia says
> In object-oriented design, the chain-of-responsibility pattern is a design pattern consisting of a source of command objects and a series of processing objects. Each processing object contains logic that defines the types of command objects that it can handle the rest are passed to the next processing object in the chain.

**Programmatic Example**

Translating our account example above. First of all we have a base account having the logic for chaining the accounts together and some accounts

```js
class Account {

    setNext(account) {
        this.successor = account
    }
    
    pay(amountToPay) {
        if (this.canPay(amountToPay)) {
            console.log(`Paid ${amountToPay} using ${this.name}`)
        } else if (this.successor) {
            console.log(`Cannot pay using ${this.name}. Proceeding...`)
            this.successor.pay(amountToPay)
        } else {
            console.log('None of the accounts have enough balance')
        }
    }
    
    canPay(amount) {
        return this.balance >= amount
    }
}

class Bank extends Account {
    constructor(balance) {
        super()
        this.name = 'bank'
        this.balance = balance
    }
}

class Paypal extends Account {
    constructor(balance) {
        super()        
        this.name = 'Paypal'
        this.balance = balance
    }
}

class Bitcoin extends Account {
    constructor(balance) {
        super()        
        this.name = 'bitcoin'
        this.balance = balance
    }
}
```

Now let's prepare the chain using the links defined above (i.e. Bank, Paypal, Bitcoin)

```js
// Let's prepare a chain like below
//      bank.paypal.bitcoin
//
// First priority bank
//      If bank can't pay then paypal
//      If paypal can't pay then bit coin

const bank = new Bank(100)          // Bank with balance 100
const paypal = new Paypal(200)      // Paypal with balance 200
const bitcoin = new Bitcoin(300)    // Bitcoin with balance 300

bank.setNext(paypal)
paypal.setNext(bitcoin)

// Let's try to pay using the first priority i.e. bank
bank.pay(259)

// Output will be
// ==============
// Cannot pay using bank. Proceeding ..
// Cannot pay using paypal. Proceeding ..: 
// Paid 259 using Bitcoin!
```

👮 Command
-------

Real world example
> A generic example would be you ordering a food at restaurant. You (i.e. `Client`) ask the waiter (i.e. `Invoker`) to bring some food (i.e. `Command`) and waiter simply forwards the request to Chef (i.e. `Receiver`) who has the knowledge of what and how to cook. 
> Another example would be you (i.e. `Client`) switching on (i.e. `Command`) the television (i.e. `Receiver`) using a remote control (`Invoker`).

In plain words
> Allows you to encapsulate actions in objects. The key idea behind this pattern is to provide the means to decouple client from receiver.

Wikipedia says
> In object-oriented programming, the command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.

**Programmatic Example**

First of all we have the receiver that has the implementation of every action that could be performed
```js
// Receiver
class Bulb {
    turnOn() {
        console.log('Bulb has been lit')
    }
    
    turnOff() {
        console.log('Darkness!')
    }
}
```
then we have an interface that each of the commands are going to implement and then we have a set of commands
```js
/*
Command interface :

    execute()
    undo()
    redo()
*/

// Command
class TurnOnCommand {
    constructor(bulb) {
        this.bulb = bulb
    }
    
    execute() {
        this.bulb.turnOn()
    }
    
    undo() {
        this.bulb.turnOff()
    }
    
    redo() {
        this.execute()
    }
}

class TurnOffCommand {
    constructor(bulb) {
        this.bulb = bulb
    }
    
    execute() {
        this.bulb.turnOff()
    }
    
    undo() {
        this.bulb.turnOn()
    }
    
    redo() {
        this.execute()
    }
}
```
Then we have an `Invoker` with whom the client will interact to process any commands
```js
// Invoker
class RemoteControl {
    submit(command) {
        command.execute()
    }
}
```
Finally let's see how we can use it in our client
```js
const bulb = new Bulb()

const turnOn = new TurnOnCommand(bulb)
const turnOff = new TurnOffCommand(bulb)

const remote = new RemoteControl()
remote.submit(turnOn) // Bulb has been lit!
remote.submit(turnOff) // Darkness!
```

Command pattern can also be used to implement a transaction based system. Where you keep maintaining the history of commands as soon as you execute them. If the final command is successfully executed, all good otherwise just iterate through the history and keep executing the `undo` on all the executed commands. 

➿ Iterator
--------

Real world example
> An old radio set will be a good example of iterator, where user could start at some channel and then use next or previous buttons to go through the respective channels. Or take an example of MP3 player or a TV set where you could press the next and previous buttons to go through the consecutive channels or in other words they all provide an interface to iterate through the respective channels, songs or radio stations.  

In plain words
> It presents a way to access the elements of an object without exposing the underlying presentation.

Wikipedia says
> In object-oriented programming, the iterator pattern is a design pattern in which an iterator is used to traverse a container and access the container's elements. The iterator pattern decouples algorithms from containers in some cases, algorithms are necessarily container-specific and thus cannot be decoupled.

**Programmatic example**
 Translating our radio stations example from above. First of all we have `RadioStation`

```js
class RadioStation {
    constructor(frequency) {
        this.frequency = frequency    
    }
    
    getFrequency() {
        return this.frequency
    }
}
```
Then we have our iterator

```js
class StationList {
    constructor(){
        this.stations = []
    }

    addStation(station) {
        this.stations.push(station)
    }
    
    removeStation(toRemove) {
        const toRemoveFrequency = toRemove.getFrequency()
        this.stations = this.stations.filter(station => {
            return station.getFrequency() !== toRemoveFrequency
        })
    }
}
```
And then it can be used as
```js
const stationList = new StationList()

stationList.addStation(new RadioStation(89))
stationList.addStation(new RadioStation(101))
stationList.addStation(new RadioStation(102))
stationList.addStation(new RadioStation(103.2))

stationList.stations.forEach(station => console.log(station.getFrequency()))

stationList.removeStation(new RadioStation(89)) // Will remove station 89
```

👽 Mediator
========

Real world example
> A general example would be when you talk to someone on your mobile phone, there is a network provider sitting between you and them and your conversation goes through it instead of being directly sent. In this case network provider is mediator. 

In plain words
> Mediator pattern adds a third party object (called mediator) to control the interaction between two objects (called colleagues). It helps reduce the coupling between the classes communicating with each other. Because now they don't need to have the knowledge of each other's implementation. 

Wikipedia says
> In software engineering, the mediator pattern defines an object that encapsulates how a set of objects interact. This pattern is considered to be a behavioral pattern due to the way it can alter the program's running behavior.

**Programmatic Example**

Here is the simplest example of a chat room (i.e. mediator) with users (i.e. colleagues) sending messages to each other. 

First of all, we have the mediator i.e. the chat room 

```js
// Mediator
class ChatRoom {
    showMessage(user, message) {
        const time = new Date()
        const sender = user.getName()

        console.log(time + '[' + sender + ']:' + message)
    }
}
```

Then we have our users i.e. colleagues
```js
class User {
    constructor(name, chatMediator) {
        this.name = name
        this.chatMediator = chatMediator
    }
    
    getName() {
        return this.name
    }
    
    send(message) {
        this.chatMediator.showMessage(this, message)
    }
}
```
And the usage
```js
const mediator = new ChatRoom()

const john = new User('John Doe', mediator)
const jane = new User('Jane Doe', mediator)

john.send('Hi there!')
jane.send('Hey!')

// Output will be
// Feb 14, 10:58 [John]: Hi there!
// Feb 14, 10:58 [Jane]: Hey!
```

💾 Memento
-------
Real world example
> Take the example of calculator (i.e. originator), where whenever you perform some calculation the last calculation is saved in memory (i.e. memento) so that you can get back to it and maybe get it restored using some action buttons (i.e. caretaker). 

In plain words
> Memento pattern is about capturing and storing the current state of an object in a manner that it can be restored later on in a smooth manner.

Wikipedia says
> The memento pattern is a software design pattern that provides the ability to restore an object to its previous state (undo via rollback).

Usually useful when you need to provide some sort of undo functionality.

**Programmatic Example**

Lets take an example of text editor which keeps saving the state from time to time and that you can restore if you want.

First of all we have our memento object that will be able to hold the editor state

```js
class EditorMemento {
    constructor(content) {
        this._content = content
    }
    
    getContent() {
        return this._content
    }
}
```

Then we have our editor i.e. originator that is going to use memento object

```js
class Editor {
    constructor(){
        this._content = ''
    }
    
    type(words) {
        this._content = this._content + ' ' + words
    }
    
    getContent() {
        return this._content
    }
    
    save() {
        return new EditorMemento(this._content)
    }
    
    restore(memento) {
        this._content = memento.getContent()
    }
}
```

And then it can be used as 

```js
const editor = new Editor()

// Type some stuff
editor.type('This is the first sentence.')
editor.type('This is second.')

// Save the state to restore to : This is the first sentence. This is second.
const saved = editor.save()

// Type some more
editor.type('And this is third.')

// Output: Content before Saving
console.log(editor.getContent())// This is the first sentence. This is second. And this is third.

// Restoring to last saved state
editor.restore(saved)

console.log(editor.getContent()) // This is the first sentence. This is second.
```

😎 Observer
--------

(Otherwise known as _"pub-sub"_)

Real world example
> A good example would be the job seekers where they subscribe to some job posting site and they are notified whenever there is a matching job opportunity.   

In plain words
> Defines a dependency between objects so that whenever an object changes its state, all its dependents are notified.

Wikipedia says
> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.

**Programmatic example**

Translating our example from above. First of all we have job seekers that need to be notified for a job posting
```js
const JobPost = title => ({
    title: title
})

class JobSeeker {
    constructor(name) {
        this._name = name
    }

    notify(jobPost) {
        console.log(this._name, 'has been notified of a new posting :', jobPost.title)
    }
}
```
Then we have our job postings to which the job seekers will subscribe
```js
class JobBoard {
    constructor() {
        this._subscribers = []
    }

    subscribe(jobSeeker) {
        this._subscribers.push(jobSeeker)
    }

    addJob(jobPosting) {
        this._subscribers.forEach(subscriber => {
            subscriber.notify(jobPosting)
        })
    }
}
```
Then it can be used as
```js
// Create subscribers
const jonDoe = new JobSeeker('John Doe')
const janeDoe = new JobSeeker('Jane Doe')
const kaneDoe = new JobSeeker('Kane Doe')

// Create publisher and attach subscribers
const jobBoard = new JobBoard()
jobBoard.subscribe(jonDoe)
jobBoard.subscribe(janeDoe)

// Add a new job and see if subscribers get notified
jobBoard.addJob(JobPost('Software Engineer'))

// Output
// John Doe has been notified of a new posting : Software Engineer
// Jane Doe has been notified of a new posting : Software Engineer
```

🏃 Visitor
-------
Real world example
> Consider someone visiting Dubai. They just need a way (i.e. visa) to enter Dubai. After arrival, they can come and visit any place in Dubai on their own without having to ask for permission or to do some leg work in order to visit any place here just let them know of a place and they can visit it. Visitor pattern let's you do just that, it helps you add places to visit so that they can visit as much as they can without having to do any legwork.

In plain words
> Visitor pattern let's you add further operations to objects without having to modify them.

Wikipedia says
> In object-oriented programming and software engineering, the visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to existing object structures without modifying those structures. It is one way to follow the open/closed principle.

**Programmatic example**

Let's take an example of a zoo simulation where we have several different kinds of animals and we have to make them Sound. Let's translate this using visitor pattern 

We have our implementations for the animals
```js
class Monkey {
    shout() {
        console.log('Ooh oo aa aa!')
    }

    accept(operation) {
        operation.visitMonkey(this)
    }
}

class Lion {
    roar() {
        console.log('Roaaar!')
    }
    
    accept(operation) {
        operation.visitLion(this)
    }
}

class Dolphin {
    speak() {
        console.log('Tuut tuttu tuutt!')
    }
    
    accept(operation) {
        operation.visitDolphin(this)
    }
}
```
Let's implement our visitor
```js
const speak = {
    visitMonkey(monkey){
        monkey.shout()
    },
    visitLion(lion){
        lion.roar()
    },
    visitDolphin(dolphin){
        dolphin.speak()
    }
}
```

And then it can be used as
```js
const monkey = new Monkey()
const lion = new Lion()
const dolphin = new Dolphin()

monkey.accept(speak)    // Ooh oo aa aa!    
lion.accept(speak)      // Roaaar!
dolphin.accept(speak)   // Tuut tutt tuutt!
```
We could have done this simply by having a inheritance hierarchy for the animals but then we would have to modify the animals whenever we would have to add new actions to animals. But now we will not have to change them. For example, let's say we are asked to add the jump behavior to the animals, we can simply add that by creating a new visitor i.e.

```js
const jump = {
    visitMonkey(monkey) {
        console.log('Jumped 20 feet high! on to the tree!')
    },
    visitLion(lion) {
        console.log('Jumped 7 feet! Back on the ground!')
    },
    visitDolphin(dolphin) {
        console.log('Walked on water a little and disappeared')
    }
}
```
And for the usage
```js
monkey.accept(speak)   // Ooh oo aa aa!
monkey.accept(jump)    // Jumped 20 feet high! on to the tree!

lion.accept(speak)     // Roaaar!
lion.accept(jump)      // Jumped 7 feet! Back on the ground! 

dolphin.accept(speak)  // Tuut tutt tuutt! 
dolphin.accept(jump)   // Walked on water a little and disappeared
```

💡 Strategy
--------

Real world example
> Consider the example of sorting, we implemented bubble sort but the data started to grow and bubble sort started getting very slow. In order to tackle this we implemented Quick sort. But now although the quick sort algorithm was doing better for large datasets, it was very slow for smaller datasets. In order to handle this we implemented a strategy where for small datasets, bubble sort will be used and for larger, quick sort.

In plain words
> Strategy pattern allows you to switch the algorithm or strategy based upon the situation.

Wikipedia says
> In computer programming, the strategy pattern (also known as the policy pattern) is a behavioural software design pattern that enables an algorithm's behavior to be selected at runtime.

**Programmatic example**

Translating our example from above, we can easily implement this strategy in javascript using its feature of first class functions.

```js
const bubbleSort = dataset => {
    console.log('Sorting with bubble sort')
    // ...
    // ...
    return dataset
}

const quickSort = dataset => {
    console.log('Sorting with quick sort')
    // ...
    // ...
    return dataset
}
```

And then we have our client that is going to use any strategy
```js
const sorter = dataset => {
    if(dataset.length > 5){
        return quickSort
    } else {
        return bubbleSort
    }
}
```
And it can be used as
```js
const longDataSet = [1, 5, 4, 3, 2, 8]
const shortDataSet = [1, 5, 4]

const sorter1 = sorter(longDataSet)
const sorter2 = sorter(shortDataSet)

sorter1(longDataSet) // Output : Sorting with quick sort
sorter2(shortDataSet) // Output : Sorting with bubble sort
```

💢 State
-----
Real world example
> Imagine you are using some drawing application, you choose the paint brush to draw. Now the brush changes it's behavior based on the selected color i.e. if you have chosen red color it will draw in red, if blue then it will be in blue etc.  

In plain words
> It lets you change the behavior of a class when the state changes.

Wikipedia says
> The state pattern is a behavioral software design pattern that implements a state machine in an object-oriented way. With the state pattern, a state machine is implemented by implementing each individual state as a derived class of the state pattern interface, and implementing state transitions by invoking methods defined by the pattern's superclass.
> The state pattern can be interpreted as a strategy pattern which is able to switch the current strategy through invocations of methods defined in the pattern's interface

**Programmatic example**

Let's take an example of text editor, it let's you change the state of text that is typed i.e. if you have selected bold, it starts writing in bold, if italic then in italics etc.

First of all we have our transformation functions

```js
const upperCase = inputString => inputString.toUpperCase()
const lowerCase = inputString => inputString.toLowerCase()
const defaultTransform = inputString => inputString
```
Then we have our editor
```js
class TextEditor {
    constructor(transform) {
        this._transform = transform
    }
    
    setTransform(transform) {
        this._transform = transform
    }
    
    type(words) {
        console.log(this._transform(words))
    }
}
```
And then it can be used as
```js
const editor = new TextEditor(defaultTransform)

editor.type('First line')

editor.setTransform(upperCase)

editor.type('Second line')
editor.type('Third line')

editor.setTransform(lowerCase)

editor.type('Fourth line')
editor.type('Fifth line')

// Output:
// First line
// SECOND LINE
// THIRD LINE
// fourth line
// fifth line
```

📒 Template Method
---------------

Real world example
> Suppose we are getting some house built. The steps for building might look like 
> - Prepare the base of house
> - Build the walls
> - Add roof
> - Add other floors
> The order of these steps could never be changed i.e. you can't build the roof before building the walls etc but each of the steps could be modified for example walls can be made of wood or polyester or stone.

In plain words
> Template method defines the skeleton of how certain algorithm could be performed but defers the implementation of those steps to the children classes.

Wikipedia says
> In software engineering, the template method pattern is a behavioral design pattern that defines the program skeleton of an algorithm in an operation, deferring some steps to subclasses. It lets one redefine certain steps of an algorithm without changing the algorithm's structure.

**Programmatic Example**

Imagine we have a build tool that helps us test, lint, build, generate build reports (i.e. code coverage reports, linting report etc) and deploy our app on the test server.

First of all we have our base class that specifies the skeleton for the build algorithm
```js
class Builder {
    // Template method 
    build() {
        this.test()
        this.lint()
        this.assemble()
        this.deploy()
    }
}
```

Then we can have our implementations

```js
class AndroidBuilder extends Builder {
    test() {
        console.log('Running android tests')
    }
    
    lint() {
        console.log('Linting the android code')
    }
    
    assemble() {
        console.log('Assembling the android build')
    }
    
    deploy() {
        console.log('Deploying android build to server')
    }
}

class IosBuilder extends Builder {
    test() {
        console.log('Running ios tests')
    }
    
    lint() {
        console.log('Linting the ios code')
    }
    
    assemble() {
        console.log('Assembling the ios build')
    }
    
    deploy() {
        console.log('Deploying ios build to server')
    }
}
```
And then it can be used as

```js
const androidBuilder = new AndroidBuilder()
androidBuilder.build()

// Output:
// Running android tests
// Linting the android code
// Assembling the android build
// Deploying android build to server

const iosBuilder = new IosBuilder()
iosBuilder.build()

// Output:
// Running ios tests
// Linting the ios code
// Assembling the ios build
// Deploying ios build to server
```

## 🚦 Wrap Up Folks

And that about wraps it up. I will continue to improve this, so you might want to watch/star this repository to revisit. Also, I have plans on writing the same about the architectural patterns, stay tuned for it.

## 👬 Contribution

- Report issues
- Open pull request with improvements
- Spread the word

## License
MIT © [Soham Kamani](http://sohamkamani.com)
Based on ["Design patterns for humans"](https://github.com/kamranahmedse/design-patterns-for-humans) Copyright 2017 Kamran Ahmed