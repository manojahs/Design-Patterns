# Design-Patterns
```
Types of Design Patterns
There are three main categories of design patterns:

1️⃣ Creational Patterns → Deals with object creation (e.g., Factory, Singleton).
2️⃣ Structural Patterns → Deals with class relationships (e.g., Adapter, Decorator).
3️⃣ Behavioral Patterns → Deals with object interaction (e.g., Observer, Strategy).


1) Singleton -> Only one instance a class exists in the entire application
----------------
Real-world example: Government / Prime Minister → Only one at a time, globally accessible.

Easy Singleton (Lazy Initialization)
--------------------------------------

public sealed class Singleton
{
    // Step 1: Create a static instance
    private static Singleton _instance;

    // Step 2: Make constructor private so no one can create object using "new"
    private Singleton() { }

    // Step 3: Provide a global access point
    public static Singleton Instance
    {
        get
        {
            if (_instance == null)   // create only if not created
                _instance = new Singleton();
            return _instance;
        }
    }

    public void ShowMessage()
    {
        Console.WriteLine("Hello from Singleton!");
    }
}

// Usage
class Program
{
    static void Main()
    {
        var obj1 = Singleton.Instance;
        var obj2 = Singleton.Instance;

        obj1.ShowMessage();

        Console.WriteLine(obj1 == obj2); // True (both are same instance)
    }
}


🔑 Key Points (Easy Explanation for Interview):
Private constructor → prevents new Singleton().
Static variable → holds only one object.
Static property → returns that single object.
When you call Singleton.Instance, it always gives the same object.

✅ Thread-Safe Singleton (with Lazy<T>)
----------------------------------------
public sealed class Singleton
{
    // Step 1: Lazy<T> ensures thread-safety automatically
    private static readonly Lazy<Singleton> _instance =
        new Lazy<Singleton>(() => new Singleton());

    // Step 2: Private constructor so no one can create with "new"
    private Singleton() { }

    // Step 3: Global access point
    public static Singleton Instance => _instance.Value;

    public void ShowMessage()
    {
        Console.WriteLine("Hello from Thread-Safe Singleton!");
    }
}

✅ Thread-Safe Singleton (Lock-based)
----------------------------------------
Only one thread will enter multiple thread cannot enter thats why we have used lock

public sealed class Singleton
{
    // Step 1: Create a static instance variable
    private static Singleton _instance;
    private static readonly object _lock = new object();

    // Step 2: Private constructor so no one can create object with "new"
    private Singleton() { }

    // Step 3: Public static property with thread safety
    public static Singleton Instance
    {
        get
        {
            lock (_lock) // ensures only one thread at a time can enter
            {
                if (_instance == null)
                    _instance = new Singleton();
                return _instance;
            }
        }
    }

    public void ShowMessage()
    {
        Console.WriteLine("Hello from Thread-Safe Singleton (lock version)!");
    }
}

4) Eager Initialization
-------------------------
public sealed class Singleton
{
    private static readonly Singleton _instance = new Singleton();
    private Singleton() { }
    public static Singleton Instance => _instance;
}

5) Double Check Locking
--------------------------

public sealed class Singleton
{
    private static Singleton _instance;
    private static readonly object _lock = new object();
    private Singleton() { }
    public static Singleton Instance
    {
        get
        {
            if (_instance == null)   // first check (no locking)
            {
                lock (_lock)
                {
                    if (_instance == null) // second check (with locking)
                        _instance = new Singleton();
                }
            }
            return _instance;
        }
    }
}

Simple Factory Pattern
-------------------
Encapsulation → Hides object creation logic.
Loose Coupling → Client depends on abstraction, not concrete classes.
Open/Closed Principle → Easy to add new product types without changing client code.

public interface  IShape
{
    public void Draw();
}

public class Square : IShape
{
    public  void Draw()
    {
        Console.WriteLine("This is Square");
    }
}

public class Cube : IShape
{
    public  void Draw()
    {
        Console.WriteLine("This is Cube");
    }
}

public class Program
{ 
    public static IShape GetShape(string type)
    {

        switch (type)
        {
            case "Square":
                return new Square();
            case "Cube":
                return new Cube();
            default:
                throw new ArgumentException("Invalid shape type");

        }
    }
}

class Test
{
    public static void Main()
    {
        IShape p = Program.GetShape("Square");
        p.Draw();

        IShape s1 = Program.GetShape("Cube");
        s1.Draw();
    }
}
--------------------

Abstract Factory
-------------------------
Abstract Factory is a creational pattern that provides an interface to create families of related objects without specifying their concrete classes.

Abstract Factory – Car Example
We’ll create two families of cars: ElectricCar and PetrolCar.
Each family has two types: Sedan and SUV.


public interface ISedan
{
    public void Drive();
}

public interface ISuv
{ 
    public void OffRoadDrive();
}

public class ElectricCar : ISedan, ISuv
{
    public void Drive()
    {
        Console.WriteLine("Driving electric car on the road.");
    }
    public void OffRoadDrive()
    {
        Console.WriteLine("Driving electric car off-road.");
    }
}

public class PetrolCar : ISedan, ISuv
{
    public void Drive()
    {
        Console.WriteLine("Driving petrol car on the road.");
    }
    public void OffRoadDrive()
    {
        Console.WriteLine("Driving petrol car off-road.");
    }
}

//Abstract Factory
public interface ICarFactory
{
   public ISedan CreateSedan();
   public ISuv CreateSuv();
}

public class ElectricCarFactory : ICarFactory
{
    public ISedan CreateSedan()
    {
        return new ElectricCar();
    }
    public ISuv CreateSuv()
    {
        return new ElectricCar();
    }
}

public class PetrolCarFactory : ICarFactory
{
    public ISedan CreateSedan()
    {
        return new PetrolCar();
    }
    public ISuv CreateSuv()
    {
        return new PetrolCar();
    }
}

public class Program
{
    public static void Main()
    {
        ICarFactory c = new ElectricCarFactory();
        ISedan sedan = c.CreateSedan();
        sedan.Drive();
        ISuv suv = c.CreateSuv();
        suv.OffRoadDrive();

        c = new PetrolCarFactory();
        sedan = c.CreateSedan();
        sedan.Drive();
        suv = c.CreateSuv();
        suv.OffRoadDrive();

    }
}

Factory Method
----------------

✅ Definition

Factory Method is a creational pattern.
It defines an interface for creating an object, but lets subclasses decide which concrete object to create.
Produces one product at a time, not a family of products.

public interface IShape
{
    public void Draw();
}

public class Circle : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing a Circle");
    }
}
public class Square : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing a Square");
    }
}

public abstract class Factory
{
    public abstract IShape CreateShape();
}

public class CreateCircle : Factory
{
    public override IShape CreateShape()
    {
       return new Circle();
    }
}

public class CreateSquare : Factory
{
    public override IShape CreateShape()
    {
        return new Square();
    }
}

public class Program
{
    public static void Main()
    {
        Factory factory = new CreateCircle();
        IShape shape = factory.CreateShape();
        shape.Draw();
        factory = new CreateSquare();
        shape = factory.CreateShape();
        shape.Draw();
    }
}

Strategy Pattern
----------------------
Defines a family of algorithms (strategies).
Encapsulates each one.
Makes them interchangeable at runtime without changing the main logic.

Imagine you’re building an e-commerce app.
A customer can pay using:
Credit Card
PayPal
Google Pay

Instead of writing if-else for each payment type, we use Strategy Pattern.

public interface IStrategy
{
    public void Pattern(decimal amount);
}

public class GooglePay : IStrategy
{
    public void Pattern(decimal amount)
    {
        Console.WriteLine($"Google pay Amount is " + amount);
    }
}
public class CreditCard : IStrategy
{
    public void Pattern(decimal amount)
    {
        Console.WriteLine($"Credit card Amount is " + amount);
    }
}

public class ShoppingCart
{
    private IStrategy _strategy;

    public void setStrategy(IStrategy strategy)
    {
        _strategy = strategy;
    }
    public void Checkout(decimal Amount)
    {
        _strategy.Pattern(Amount);
    }

}

public class  Program
{
    public static void Main()
    {
       ShoppingCart cart = new ShoppingCart();
       cart.setStrategy(new GooglePay());
       cart.Checkout(10);

       cart.setStrategy(new CreditCard());
       cart.Checkout(200);
    }
    
}















```

