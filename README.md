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

// Usage
class Program
{
    static void Main()
    {
        var obj1 = Singleton.Instance;
        var obj2 = Singleton.Instance;

        obj1.ShowMessage();
        Console.WriteLine(obj1 == obj2); // True (always same object)
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

// Usage
class Program
{
    static void Main()
    {
        var obj1 = Singleton.Instance;
        var obj2 = Singleton.Instance;

        obj1.ShowMessage();
        Console.WriteLine(obj1 == obj2); // True
    }
}















problem
----------

public class EmailNotification
{
    public void Send(string message)
    {
        Console.WriteLine($"Email: {message}");
    }
}

public class SMSNotification
{
    public void Send(string message)
    {
        Console.WriteLine($"SMS: {message}");
    }
}
Problem is suppose we are creating notification service now we have a question whether to use emailnotification or smsnotification

Here mainly we use Open closed principle

✅ Factory Pattern: A Better Approach
------------------------------------------
public interface INotification
{
    void Send(string message);
}

public class EmailNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"Email: {message}");
    }
}

public class SMSNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine($"SMS: {message}");
    }
}

3️⃣ Step 3: Create a Factory to Generate Objects

public class NotificationFactory
{
    public static INotification CreateNotification(string type)
    {
        if (type.Equals("Email", StringComparison.OrdinalIgnoreCase))
        {
            return new EmailNotification();
        }
        else if (type.Equals("SMS", StringComparison.OrdinalIgnoreCase))
        {
            return new SMSNotification();
        }
        else
        {
            throw new Exception("Invalid notification type.");
        }
    }
}
4️⃣ Step 4: Use the Factory in the Main Code
class Program
{
    static void Main()
    {
        INotification notification = NotificationFactory.CreateNotification("Email");
        notification.Send("Hello, Factory Pattern!");

        INotification smsNotification = NotificationFactory.CreateNotification("SMS");
        smsNotification.Send("Hello via SMS!");
    }
}

```

