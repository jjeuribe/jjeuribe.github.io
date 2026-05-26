---
title: OOP - Classes - The Clay That Shapes Your Software
image:
  path: assets/2026-05-26-oop-classes-the-clay-that-shapes-your-software/8xr24p8xr24p8xr2.png
date: 2026-05-26 07:00:00 -0600
categories: [Software Design, OOP]
tags: [classes]     # TAG names should always be lowercase
---

I've always seen OOP (Object-Oriented Programming) as the closest thing developers have to playing God. You essentially create worlds from scratch, you're constantly shaping a small version of reality inside your software: A banking system has accounts, transactions, and customers. A game has players, weapons, missions, and enemies. An e-commerce platform has carts, products, and orders. This is what OOP is really all about: _building software that reflects the structure and behavior of the real world_, and **Classes** are the first building blocks that make this possible. 

If you've spent some time digging into OOP, you likely already know what a class actually looks like:

```ts
class Player {
  private health: number = 100;
	
  constructor(
    public readonly id: string,  
    public readonly name: string, 
    public readonly rank: string
  ) {}
	
  public attack(): void {
    if (!this.isAlive()) {
      throw new Error(`${this.name} is dead and cannot attack`);
    }
    
    // Perform logic to attack the enemy
    // ...
  }
	
  public receiveDamage(damage: number): void {
    if (damage <= 0) {  
      throw new Error('Damage must be greater than zero');  
    }
    
    this.health -= damage;
    
    if (this.health <= 0) {  
      this.health = 0; 
    }
  }
	
  public respawn(): void {
    this.health = 100;
  }

  public isAlive(): boolean { 
    return this.health > 0;
  }
}
```

As you can see, a class is basically a **_"blueprint"_** that groups together **properties** and **methods**, and which can be used to create objects from it. Some people also describe classes as simple containers used to organize your code. 

And while those definitions aren't wrong, they feel a bit too simplistic. You see, as soon as you start modeling real business problems, classes stop feeling like simple **_"blueprints"_** and start behaving more like living creatures inside your software, actors with responsibilities.

In my head, a class is essentially a piece of **clay**.

With **clay**, a skilled craftsman can shape meaningful objects from it. In software, classes work the same way, they let you shape and represent real-world entities directly in your code. 

But simply having clay doesn't automatically make a great sculpture. The quality of the result depends on the craftsman’s understanding, technique, and intention behind the modeling process. In this sense, classes are no different.

Good class design doesn't come from creating abstractions by accident or guessing structures. It comes from understanding the **business requirements**: the rules, constraints, behaviors, and relationships that exist in the real world, and then translating that understanding into clear, intentional software models.

To make this easier to visualize, let’s go through the following example.

## The Starting Point - Business Requirements

Imagine you're part of the development team behind **_Helldivers 2_**, and one day you receive the following business requirements:

> **Business requirement fragment:**
> * Players participate in cooperative missions.
> * Each mission contains objectives that must be completed.
> * Players can die during missions.
> * Dead players cannot attack.
> * Squads may reinforce dead players a limited number of times.
> * Missions fail if all players are dead and no reinforcements remain.
> * Missions succeed when all objectives are completed.
{: .prompt-info }

At this point, your job as a developer is not to immediately jump into writing code or start thinking about databases, tables, UI screens, or APIs. You first need to understand what those business requirements are actually telling you.

## From Requirements to Domain Concepts

You can begin by identifying the most important **business concepts** hiding inside the **business requirements**. 

A simple trick here is to pay attention to **nouns**, because they usually end up becoming great candidates for _domain concepts_ and, eventually, classes. Let's break it down:

![Desktop View](/assets/2026-05-26-oop-classes-the-clay-that-shapes-your-software/20260526103643.png)

Without even writing a single line of code yet, you've already spotted several _domain concepts_ from the game domain itself, things like `Player`, `Mission`, `Objective`, `Squad`, etc. That is your **business domain** taking shape.

![Desktop View](/assets/2026-05-26-oop-classes-the-clay-that-shapes-your-software/20260514132357.png)

Later on, many of these concepts may evolve into classes that will help you model the rules, behavior, and structure of your system.

## Understanding the Business Logic

The **business domain** is not only about _nouns_, it also contains **business logic**, and business logic is mostly made up of **Rules & Constraints**. 

While **rules** define how your system behaves, **constraints** ensure that your system never enters an invalid state. 

You can identify both from the business requirements as well. Let's see:

![Desktop View](/assets/2026-05-26-oop-classes-the-clay-that-shapes-your-software/20260526103926.png)

This part is extremely important because classes are not just there to hold data. They also encapsulate behavior in the form of these rules and constraints, ensuring your **domain model** remains consistent, predictable and reliable.

## Building the Domain Model - From Concepts To Code

While the **business domain** is the story of your application, the **domain model** is how you bring that story to life in code through classes, behavior, rules, and relationships working together. 

The key idea is that classes should not just store data. They should also **enforce and protect the rules and constraints of the domain**. 

With this in mind, Let's start modeling the system.

### The Player Class

From the requirements, you already know that: 

1. A player can attack. (rule)
2. A player can die. (rule)
3. Dead players cannot attack. (constraint)

The next step is simply turning those rules and constraints into code:

```ts
class Player {
  private health: number = 100;

  constructor(
    public readonly id: string,  
    public readonly name: string, 
    public readonly rank: string
  ) {
    if (!id || id.trim().length === 0) {  
      throw new Error('Player id is required');  
    }  

    if (!name || name.trim().length === 0) {  
      throw new Error('Player name is required');  
    }  

    if (!rank || rank.trim().length === 0) {  
      throw new Error('Player rank is required');  
    }
  }

  public attack(): void {
    if (!this.isAlive()) {
      throw new Error(`${this.name} is dead and cannot attack`);
    }
    
    // Perform logic to attack the enemy.
  }

  public receiveDamage(damage: number): void {
    if (damage <= 0) {  
      throw new Error('Damage must be greater than zero');  
    }
    
    this.health -= damage;
    
    if (this.health <= 0) {  
      this.health = 0; 
    }
  }

  public respawn(): void {
    this.health = 100;
  }

  public isAlive(): boolean { 
    return this.health > 0;
  }
}
```

You can see how the business logic and boundaries live inside the class. The `Player` is responsible for protecting its own rules and enforcing its constraints, so nothing outside needs to worry about them. 

The idea is to keep the class clean, cohesive, and aligned with a _single responsibility_.

### The Objective Class

The requirement says that _a mission can contain several objectives, and those objectives must be completed_. That means an `Objective` is not just a piece of text, it also has **behavior** and **state**. So, why not giving it its own class:

```ts
class Objective {
  private completed = false; 

  constructor(public readonly description: string) {
    if (!description || description.trim().length === 0) {  
      throw new Error('Objective description is required');  
    }
  }

  public complete(): void {
    this.completed = true;
  }

  public isComplete(): boolean {
    return this.completed;
  }
}
```

It may look simple at first, but it already captures an important business rule: _an objective controls its own completion state_.

### The ReinforcementSystem Class

The reinforcement system is another good example of how classes are responsible for protecting rules, not just storing values. Let's take a look. 

From the business requirements, you know two things. 

1. Reinforcements are limited. 
2. You can only call for reinforcements as long as there are any left. 

In this sense, instead of treating reinforcements as just a number, why not give that behavior a dedicated place:

```ts
class ReinforcementSystem {

  constructor(private remainingReinforcements: number) {
    if (this.remainingReinforcements < 0) {
      throw new Error('Reinforcements cannot be negative')
    }
  }

  public callReinforcement(): void {
    if (!this.canReinforce()) {
      throw new Error('No reinforcements remaining');
    }
    
    this.remainingReinforcements--;
  }

  public canReinforce(): boolean {
    return this.remainingReinforcements > 0; 
  }
    
  public getRemainingReinforcements(): number {
    return this.remainingReinforcements;  
  }
}
```

Notice how the `ReinforcementSystem` class does more than holding a number. It protects its business rules and enforces an important game constraint: _reinforcements cannot go below zero_. That constraint lives inside the model, exactly where it belongs.

At first, separating your _domain model_ into multiple classes can feel like overkill. In this situation, it’s very tempting to just throw everything into a single `Mission` class and move on:

```ts
class Mission {
  players = [];  
  objectives = [];  
  reinforcements = 5;  
}
```

This might seem like a simpler approach, but it'd quickly turn into what developers call a **God Class**: _a class that knows too much, does too much_, which becomes harder to maintain over time. Instead, a better approach is what you’ve been doing so far, separating responsibilities into focused domain concepts:

- `Player` manages player state and combat rules.
- `Objective` manages completion.
- `ReinforcementSystem` manages reinforcement rules.

Each class has a **clear responsibility**, protects its own rules and enforce its constraints. The result is a system that’s easier to understand, easier to test, and much easier to evolve as new requirements come in.

### The Squad Class

Moving on. The business requirements say that _a squad can reinforce dead players_. In this sense, the squad isn't just a collection of players, it's responsible for coordinating when and how reinforcements happen. That responsibility can be captured in its own class:

```ts
class Squad {

  constructor(
    private players: Player[],
    private reinforcementSystem: ReinforcementSystem
  ) {}

  public reinforce(player: Player): void {
    if (player.isAlive()) {
      throw new Error('Player is already alive');
    }
    
    if (!this.canReinforce()) {  
      throw new Error('No reinforcements available');  
    }
    
    this.reinforcementSystem.callReinforcement();
    player.respawn();
  }

  public canReinforce(): boolean {
    return this.reinforcementSystem.canReinforce();
  }

  public areAllPlayersDead(): boolean {
    return this.players.every(player => !player.isAlive())
  }

  public getPlayers(): Player[] { 
    return [...this.players];  
  }
}
```

As you can see, the `Squad` class is acting as an orchestrator for players and reinforcements, doing just enough without taking on extra responsibility.

### The Mission Class

Lastly, you still have a couple of business rules that define how a mission behaves:

1. A mission fails if all players are dead and no reinforcements remain.
2. A mission succeeds when all objectives are completed.

They're describing the mission itself. So it makes sense to capture them in a `Mission` class, like so:

```ts
class Mission {

  constructor(
    private objectives: Objective[], 
    private squad: Squad
  ) {}

  public isSuccessful(): boolean {
    return this.objectives.every(objective => objective.isComplete()); 
  }

  public isFailed(): boolean {
    return (this.squad.areAllPlayersDead() && !this.squad.canReinforce());
  }
}
```

Notice what's going on here: The `Mission` class don't try to know everything, instead, it delegates: 

* It asks `Objective` whether it's completed or not. 
* It asks `Squad` about player state. 

The `Mission` class is simply orchestrating the rules, not owning every detail.

## Why the Domain Model Matters

At this point you have a real **domain model**.

A `Player`, a `Squad`, a `Mission`, a `ReinforcementSystem` aren't just technical ideas, they already existed in the problem space, they came from understanding the **business requirements** and the **domain** itself. Your job was simply to discover them and translate them into code as clearly as possible.

Now, here’s something subtle but extremely important: notice that there are no databases, API Endpoints, Frameworks, or UI concerns here. That’s not by accident, it means you've isolated the **core of your system**, the part that actually makes the decisions, enforces rules, and defines behavior. 

In practice, your application will include multiple layers, but the **domain layer** will sit at the heart of your business software, everything else will depend on it, and never the other way around:

![Desktop View](/assets/2026-05-26-oop-classes-the-clay-that-shapes-your-software/20260513152112.png)

Think about it this way: 

* The UI can change (web, mobile, CLI). 
* The database can change (in-memory, PostgreSQL, MongoDB). 
* The framework can change (NestJS, Express, Laravel). 

But your business rules should remain stable. 

If your core logic depends on a framework or a database, then every technical change has the potential to become a **business risk**.  

A well-designed domain model protects you from that. It keeps your business logic _independent_, _stable_, and _reusable_, no matter how the surrounding technology changes.

## Some Best Practices for Classes

### Use Meaningful Names

**Naming** is one of those things that feels trivial until you realize it's doing most of the heavy lifting in your design. 

Your best class names will come from the **business domain**, not from programming jargon. You already did this: 

```ts
class Player {}
class Mission {}
class ReinforcementSystem {}
```

These names make sense because they reflect real concepts from the problem you're solving. 

A good rule of thumb is to use  **_nouns_** for classes, and **_verbs_** for methods: 

```ts
// Class = Noun
class Player {
  attack() {}        // Verb
  receiveDamage() {} // Verb
}
```

This creates code that reads almost like plain English, anyone outside engineering can understand it without any trouble. 

A well-named class reduces the need for comments, makes your code easier to navigate, and helps others understand the system almost instantly.

### Keep Classes Small and Focused 

A class should have a **single, clear responsibility**. Think of this like a _function_, it should do one thing, an do it well. 

Keep in mind though, when I say that a class should _"do one thing and do it well"_ or that it should be _"small"_, I'm not talking about the number of lines in the code. You can have a class that can be 10, 50, or even 100 lines long and still be perfectly fine, as long as everything inside serves a **single, cohesive responsibility**. 

What you want to avoid is something like this: 

```ts
class Mission {  
  players = [];  
  objectives = [];  
  reinforcements = 5;  
}
```

As mentioned earlier, this can easily turn into a **God Class**: a class that knows too much and does too much, and it won't be _"big"_ because of its size, but because of **mixed responsibilities**. 

In that sense,  _"small"_ doesn’t mean fewer lines, it means **focused responsibility**.

That’s why breaking things down into smaller, focused classes makes such a difference:

 * `Player` manages player state and combat rules. 
 * `Objective` manages completion. 
 * `ReinforcementSystem` manages reinforcement rules.

Remember: if everything inside the class is aligned with the same purpose, you’re on the right track, even if the class isn’t that tiny.

### Model Behavior, not just Data

A class should represent what an object can do, not just what it holds. 

Imagine having a `Player` class like this: 

```ts
class Player {  
  public health: number = 100;  
  public isAlive: boolean = true;
}
```

It might not seem obvious at first glance, but that'll quickly lead you to what's known as an **anemic model**: an object that only stores data, while all the real behavior lives somewhere else. The result?, you'll be constantly pulling data out of the object and making decisions elsewhere. Something like this:

```ts
function attack(player: Player): void {  
  if (!player.isAlive) {  
    throw new Error("Dead player cannot attack");  
  } 

  // attack logic...
}

function receiveDamage(player: Player, damage: number): void {
  if (damage <= 0) {  
    throw new Error("Invalid damage");  
  }  

  player.health -= damage;  

  if (player.health <= 0) {  
    player.health = 0;  
    player.isAlive = false;  
  }
}  
```

Can you see how the business logic lives outside the object itself?. 

When that happens, your rules and constraints will end up getting duplicated and scattered all over the system, making the code harder to maintain and much easier to break. 

So your best move is to bring that logic back to where it actually belongs: inside the object itself:

```ts
class Player {  
  private health: number = 100;
    
    public attack(): void {  
    if (!this.isAlive()) {  
      throw new Error("Dead player cannot attack");  
    }  

    // attack logic...  
  }  

  public receiveDamage(damage: number): void {  
    if (damage <= 0) {  
      throw new Error("Invalid damage");  
    }  
      
      this.health -= damage;
    
    if (this.health <= 0) {  
      this.health = 0; 
    }
  }  

  public isAlive(): boolean {  
    return this.health > 0;  
  }
}
```

Now the class owns its behavior and protects its state. Rules and constraints are all kept in one place. 

Remember: if your class is just holding data, you're not modeling a domain, you're just modeling a _"database row"_.

### Keep Constructors Simple

Avoid putting anything with latency or complex workflows inside constructors. This includes things like: 

* API calls. 
* Database queries. 
* File access. 
* Heavy calculations. 

For example, you shouldn't do something like this: 

```ts
class Player {
  constructor(public id: string) {
    const playerData = fetchPlayerFromAPI(id);
    ...
  }
}
```

At this point, creating a `Player` object depends entirely on an external system, not to mention it can fail and introduce unnecessary delays. 

Remember: constructors should create valid objects, not perform work that can fail, block, or depend on external systems. 

## Key Takeaways

* Stop thinking in classes as just _"a place to organize code"_. Instead, think of them as representations of real-world concepts. Classes are how you **speak the language of the business** in code. When you do this, your project will start to organize itself naturally.
* Classes are **guardians** of business rules and constraints. They don't just hold data, they protect invariants and make sure your system never enters an invalid state.  
* The **domain** is not your code. It exists independently in the real world. Your job is to discover it, understand it, and translate it into code. 
* Your **domain model** should be independent of frameworks or infrastructure. Frameworks will change, databases will change, but your business rules remain. 
* Don't start by thinking _"What classes should I create"?_. Instead, start with: _"What concepts actually exist in the business domain?"_.  Analyze your business requirements, understand the domain, and let your classes emerge from that understanding.  
* Well-designed classes make your system easier to understand, easier to test, easier to extend, and harder to break.
