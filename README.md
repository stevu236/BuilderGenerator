[![Nuget](https://img.shields.io/nuget/dt/Safalin.BuilderGenerator)](https://www.nuget.org/packages/Safalin.BuilderGenerator/)
[![GitHub](https://img.shields.io/github/license/safalin1/BuilderGenerator2)](https://opensource.org/licenses/MIT)
[![ci](https://github.com/safalin1/BuilderGenerator2/actions/workflows/ci.yml/badge.svg)](https://github.com/safalin1/BuilderGenerator2/actions/workflows/ci.yml)

# BuilderGenerator2 - an updated version of BuilderGenerator #

An updated version of BuilderGenerator - a .NET Source Generator by MelGrubb, that is designed to add fluent builder classes to your projects. [Builders](https://en.wikipedia.org/wiki/Builder_pattern) are an object creation pattern, similar to the [Object Mother](https://martinfowler.com/bliki/ObjectMother.html) pattern. Object Mothers and Builders are most commonly used to create objects for testing, but they can be used anywhere you want "canned" objects.

## What is new? ##

### Record type support
You can now use the `[BuilderFor]` attribute on C# record types:
```csharp
public record MyRecord(bool Hello);

[BuilderFor(typeof(MyRecord)]
public partial class MyRecordBuilder
{
}

var builder = new MyRecordBuilder().WithHello(true).Build();
```

### WithValuesFrom
The new `WithValuesFrom` method allows you to copy values from an existing instance into your builder:
```csharp
new MyBuilder().WithValuesFrom(anotherObject)
```

## Installation ##

BuilderGenerator2 is installed as an analyzer via a NuGet package.

For more info on how to install/add this package to your solution, visit nuget.org: [Safalin.BuilderGenerator](https://www.nuget.org/packages/Safalin.BuilderGenerator/)

## Usage ##

After the package has been installed into your project:

1. Create a new partial class that will hold your builder methods.
2. Decorate it with the ```BuilderFor``` attribute, specifying the type of class that the builder is meant to build. For example: 
   ```csharp
   [BuilderFor(typeof(Foo))]
   public partial class FooBuilder
   {
   }
   ``` 
3. Rebuild your project. The source generator will run and autogenerate methods in a separate partial class file, for each property in the type you specified in Step 2. 

   You can also add factory methods to your partial class which can craft specific data scenarios: 

   ```csharp
   [BuilderFor(typeof(Foo))]
   public partial class FooBuilder
   {
       public static FooBuilder Bar()
       {
           return new FooBuilder()
               .WithBar(true);
       }
       
       public static FooBuilder NotBar()
       {
           return new FooBuilder()
               .WithBar(false);
       }
   }
   ``` 

## Version History ##
- v2.0
  - Rewrote internal source generation logic
  - Added XML comments for auto-generated With methods 
  - Renamed from am4u.BuilderGenerator to Safalin.BuilderGenerator

- v1.1
  - Added `WithValuesFrom` fluent method to builders

- v1.0
  - Initial fork
  - Updated to include record type support
