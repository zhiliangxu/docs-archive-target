---
title: .NET Development Guidelines
description: Rules and suggestions for developing code on .NET for APEX
---
# .NET Development Guidelines

This document covers the rules of the road for developing .NET code for all engineers that work on projects under APEX (both vendor and FTE). If you have any feedback or questions, feel free to email Mike Sampson (misampso@microsoft.com).

## Platform Choices

All code must conform to the following platform choices unless previous approval for an exception is given:
* All code must be written in **C#**
* The latest accepted version of **.NET Core** (**2.0** as of this writing)
* The latest version of all libraries and frameworks compatible with this version (ASP.Net, Azure SDKs, etc.)
* All projects and solutions created must load in **Visual Studo 2017**
* **XUnit** is used to write unit tests 
* **Git** used for source control with **VSTS** as the central repo host
* VSTS Builds and Releases defined for both continuous integration as well as production deployment
* **Azure** used as the cloud platform (see [Azure Guidelines](azure-development-guidelines.md) for more)

## Architecture and Design

All architecture should be reviewed by the engineering team for internal projects and with the architecture lead of the project for vendor projects. This includes both high level design and usage of services as well as the general function of large components. Very simplistic services, such as simple data transforms or basic CRUD services, do not need a review of their internals.

To help prepare for the review, here are the things that the review will look for.

* **Simple** - Create the smallest number of components needed to achieve the goal. Each piece should have a clear, small purpose and collaborate with other similar objects to accomplish the task.
* **Fault Tolerant** - Random failures of underlying infrastructure, bad data, and bugs should hinder the service in the least way possible. Jobs should be rerunnable, actions undoable, and machines replaceable.
* **Monitored** - The ability to detect and diagnose errors and anomalies should be baked into the design of the system. Geneva/MDS usage is mandatory for all Azure deployed services. App Insights should be used for application level diagnostic monitoring (trace, exceptions, etc.).
* **Scalable** - The system's design should allow for a reasonable amount of scaling up of the workload. If there are choices that limit massive scale in the name of keeping things simple for now, this should be noted in case massive scale is later required.

When possible, use recognizable patterns in your architecture such as microservices, event sourcing, or CQRS.

## Coding Practices

While architecture covers the high level design and goals of the system, the coding practices cover the rules of the road for the C# code itself.

### Test First Development

All code should be written in a "test first" or "test driven development" style for all internal projects. While this has many meanings to many people, in our case it means just what it says: write a test, see it fail, fix it with code, then refactor. The resulting tests should be fast (milliseconds) and runnable both at build locally and in VSTS. Create a companion test project for each project and keep it in the solution that contains the project.

Tests must cover all classes and all methods in the project. For calls that require access to resources that cannot be provided at test time, it is allowed to create and interface and wrapper class to isolate the system from this method. These wrapper classes do not need tests as long as their logic is very simple and they remain as close to pure wrappers as possible.

When writing tests, no mocking framework should be used. When test fakes are needed, basic test versions of classes with minimal functionality will be created in the Unit Testing project. These mocks should not be shared between test projects.

For vendor projects, "test first" isn't a hard requirement but a full suite of unit tests that cover every component is required upon code delivery. Test First is recommended as this is the easiest and fastest way to achieve this but it is not required. All internal projects must be Test First, however. Writing unit tests after the fact is not permitted for internal work and it will be obvious from both the low level design and the content of the tests.

### Naming and Comments

Classes, methods, and variables should have long, descriptive names which allow the reader to determine their behavior clearly. This is sometimes referred to as "self documenting." Reading the code should be sufficient to understand what is happening in the individual class or method assuming the reader has a general understanding of the architecture.

In general, match the style of the code around you when it comes to naming private members, parameters, and the like. If the project is green field, prefer adding underscores to field names and keeping all public names in keeping with standard .NET style guidelines.

Comments are not permitted unless they indicate very non-intuitive behavior caused by a dependency. Any documentation must be external to the code and should only cover higher level topics. Unclear code should be changed until it is clear rather than adding a comment. Comments are not checked by the compiler and will never be as up to date as the source code. Readable code is paramount.

### Async and Parallel

When it is available, make use of the async versions of methods and the language constructs of C# to use them safely. Calling external services and disk access for large files should be done async when it makes sense for the work.

Try to parallelize work where possible to improve performance. Jobs that process a large number of records with no interdependence between them should use the [TPL Dataflow](https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/dataflow-task-parallel-library) library to clearly express their work as a series of linked steps.

### Immutability and Pure

Whenever possible, write simple functions that takes some inputs and produces some outputs without introducing any side effects. Side effects includes manipulating a shared state, mainpulating the input, accessing or mutating global or static state, reading additional variables from environments or files, writing outputs to files. 

Write complex functions by composing smaller, simpler functions. Write functions that takes the minimum required parameters and dependencies.

Tuples, readonly, nested functions are the tools to help write these stateless pure functions. Try returning multiple outputs with tuples over manipulating an input context.


## Security

See the [Security Guidelines](security-guidelines.md) for rules around secure monitoring, secret management, and more requirements.

### Safe deserialization

Whenever using Json.Net to deserialize data, you must set the JsonSerializerSettings.TypeNameHandling value to None. If you are using JsonConvert, you can set this once at the startup of the app by using the JsonConvert.DefaultSettings property.
