# DIContainer

A simple and lightweight dependency injection (DI) container written in C#.

## Features ✦

- Register factories and instances with optional tags.
- Support for singleton instances via `.AsSingle()`.
- Resolve dependencies with cyclic dependency detection.
- Optional hierarchical container structure (parent containers).
- Automatic disposal of `IDisposable` instances.

## Getting Started 🚀

### Installation

Just include `DIContainer.cs` and `DIEntry.cs` in your project under the `RostCont` namespace.

### Basic Usage

```csharp
using RostCont;

// Create container
var container = new DIContainer();

// Register a factory
container.RegisterFactory(c => new MyService());

// Register a singleton
container.RegisterFactory(c => new MySingletonService()).AsSingle();

// Register instance
container.RegisterInstance(new MyInstance());

// Resolve dependencies
var service = container.Resolve<MyService>();
```

### Tagged Registrations
Tags allow you to register multiple versions of the same type.

```csharp
container.RegisterFactory("v1", c => new MyServiceV1());
container.RegisterFactory("v2", c => new MyServiceV2());

var serviceV1 = container.Resolve<MyService>("v1");
var serviceV2 = container.Resolve<MyService>("v2");
```

### Nested Containers
You can create a child container that inherits all registrations from its parent.

```csharp
var parent = new DIContainer();
parent.RegisterInstance(new Config());

var child = new DIContainer(parent);
var config = child.Resolve<Config>(); // resolved from parent
```



### Error Handling
- Throws an exception if a dependency is already registered with the same tag and type.
- Detects and throws an error on cyclic dependencies.
- Throws an exception if a dependency cannot be resolved.

### Cleanup
DIContainer and its entries implement IDisposable. Call Dispose() to clean up all disposable instances.

```csharp
container.Dispose();
```

## Example 🧩

```csharp
public interface IService {}
public class Service : IService {}

public class App
{
    public App(IService service) {}
}

// Usage
var container = new DIContainer();
container.RegisterFactory(c => new Service()).AsSingle();
container.RegisterFactory(c => new App(c.Resolve<IService>()));

var app = container.Resolve<App>();
```

