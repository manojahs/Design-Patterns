# Design-Patterns

Types of Design Patterns
There are three main categories of design patterns:

1️⃣ Creational Patterns → Deals with object creation (e.g., Factory, Singleton).
2️⃣ Structural Patterns → Deals with class relationships (e.g., Adapter, Decorator).
3️⃣ Behavioral Patterns → Deals with object interaction (e.g., Observer, Strategy).


1) Singleton -> Only one instance a class exists in the entire application
----------------
Real-world example: Government / Prime Minister → Only one at a time, globally accessible.
























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



