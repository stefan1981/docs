
# SOLID Design principles

### Single Responsibility Principle (SRP)
- classes should have only specific responsibility.
- therefore it should not give several reasons to change a class.

### Open / Close Principle (OCP)
- A module should be open for extensions and closed for modifications.
- Using the "Dependency Inversion Principle" is one way to achieve the "Open Close Principle".
- OCP can reached by using the Strategy pattern

### Liskov Substitution Principle (LSP)
- A instance of a derived class (subclass) should bahave in such a way, that it can be used as the base class, without any unexpected behaviour. - A derived class should be able to replace its Base-class without causing errors.

### Interface Segregation Principle (ISP)
- An interface should be limited to a minimum of methods, so that the inheriting classes don't need to implement unneeded methods.

### Dependency Inversion Principle (DIP)
- If a class depends on another class, both of them should depend on abstractions, rather than on concrete implementations.
- Interfaces (abstractions) are less likely to change than concrete implementations, so relying on interfaces lead less likely to breaks.

# Component cohesion
### (REP) Reuse-Release-Equivalence Principle
- If you want to reuse a component beyond the scope of a certain release, you need to track its version number and its behaviour.In the best case, a release creates a comprehensive documentation, about what a component does.

### (CCP) Common-Closure Principle
- Classes which need to be modified by the same reason should be clustered into the same component.

### (CRP) Common-Reuse Principle
- Those classes who are usually used together, should be in the same component.

# Component coupling
### (ADP) Acyclic-Dependencies Principle
- There should not be any cyclical dependencies between components. In general: Specific things should depend on abstract things. Not the other direction.

### (SDP) Stable-Dependencies Principle
- Dependencies should flow into the same direction like stability.
- The Fan-In/Fan-Out Metric is a tool to measure the stability.

### (SAP) Stable-Abstraction Principle
- A component should be as abstract as stable.

# General software-development principles
### Don't repeat yourself (DRY)
Dont write the same code again and again. Try to reuse them as good as possible. By applying higher software-design concepts it will become more easy to reuse code.

### Keep it Simple Stupid (KISS)
Don't overcomplicate things. Try to keep things as simple as possible. Showing you skills with complex and hard to read algorithmns is counter productive.

### Favour Composition over Inheritance (FCol)
It is better to compose objects with others than to build structures who derive from each other. When you compose an object from other objects it's easier to replace parts and maintain.

### Boy scout rule
Always leave a place (the source-code) more tidy than you have found it.

### Root cause analysis
Find/fix the root-cause of a problem. Just solving symptoms won't eliminate the problem. Understand and solve what cause the problem and you solved it fundamentally.

### Single level of abstraction (SLA)
There are different levels of Abstraction, like shifting bits abround and calling methods. Try not to mix this different layers of abstraction.

### Seperation of Concerns (SoC)
Try to seperate different concerns from each other. SoC is closely related to the Single-Responsibility-Principle. Concerns cover broader aspects like Database-Access, Graphical-Output or Business-logic. When you have these things properly seperated, its easier to replace whole sub-systems or connect other user-interfaces. SoC leads to loose koupling and a higher cohession.

### Information Hiding Principle
The less information an object exposes the outer world (e.g. public methods), the less is its coupling to the outer world. Objects who have a limited set of public functions are more intuitive to use.Try to hide purely internal stuff inside the object and don't allow them to be used from outside.

### Law of Demeter
The law of demeter says that the inter-relation between objects should be limited to a necessary amount. Whatever this amount might be, but an object should communicate to its relatives and not to every stranger-object.

### You Ain't gonna need it (YAGNI)
Do not implement functionality that might cover unclear requirements. Before you write dozens of exceptions which try to cover side-effect, caused by unclear requirements, you better clarify the requirements and make them more strict. The requirements are the crucial part of the software-development. The more precise the requirements are, the simpler it is to develop a ruleset matching excactly this requirements.

# Domain driven development (DDD)
### Ubiquitous language
a language, developed specifically within a bounded context. This language should be developed with the domain-experts (those people who hold the knowledge about the domain) and the developers.

### Bounded Context
This is a pattern for strategic design within DDD. With a bounded context you break down larger contexts into smaller ones to reach a clear seperation between contexts. WIthin such a context you usually have a ubiquitous language. Furthermore it is important to deermini the interrelations between bounded contexts.

### sub domains
Your company is a whole large company domain. You may just work in a specific or in a logically seperated part of that company. These part can be called a subdomain. There are three kinds of subdomains: the core domain (this is where the money is generated), supporting domains (these one support your core business) and generic ones (these ones are needet but not directly connected to your business). A bounded context may have several subdomains within. In the best case a
    bounded context reflects a one-to-one relation between subdomain and bounded-context.

### Core domain
The core domain reflects the core of your company, you main business, its the heard of what you company does, the core domain should be developeed only by the brightest heads in your company.

### Context Mapping
With context mapping you define some kind of interrelation between bounded contexts. Different kinds of context mapping exist (partnership, shared kernel, custom-supplier, conformist, anticorruption-layer, open host service, published language, separate ways). Generally speak context mapping is about interfaces between bounded contexts.

### Big Ball of Mud
A big ball of mud is a large monolithic system without clear boundaries. Everithing does somehow depend on everithing. A change can possibly cause a defect somewhere else.

### Entity
An entity represents an individual object with an owb identity. For example: a specific instance of an user is an entity. A user is a general object, but a specific instance of a user is an entity.

### Value Object
A value object represents an unchangable conceptual Whole.

### Aggregate
Examples of Aggregates are Products, Discussions, Forums. They are logical units which are made (aggregated) from other parts.

### Domain Event
Domain events are events specific for a domain. The name of a domain events should contain a verb in the past-form like: productCreated, customerCalled, cakeBaked. Domain events are emited by their producers and can be imagined as messages, flowing to their recipients to trigger ongoing actions.

### Event storming
Event storming is a management technique where domain-experts an developers together design the business-processes. It is not about the technical aspects but about the processes itself.

# PHP version changes

## PHP 8.0 renewals
- Release Notes PHP 8.0
- - named arguments
- - attributes
- - constructor property promotion
- - union types (public int|float $id)
- - match expression (match)
- - nullsafe operator ( $obj?->value )
- - string to number comparison
- - consistent type error
- -
## PHP 7.4 renewals
-  Release Notes PHP 7.4
- - typed properties (public int $id)
- - arrow functions
- - ...
- - null coalescing assignment operator ( ??= )
- - unpacking inside arrays $fruits = ['banana', ...['one', two], 'melon']
- - numeric literal seperator _
- - ...

