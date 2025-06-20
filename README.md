# DIContainer

A simple and lightweight dependency injection (DI) container written in C#.

## Features

- Register factories and instances with optional tags.
- Support for singleton instances via `.AsSingle()`.
- Resolve dependencies with cyclic dependency detection.
- Optional hierarchical container structure (parent containers).
- Automatic disposal of `IDisposable` instances.

## Getting Started

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
