# Dependency Inversion Principle

Principle:
: Entities must depend on abstractions, not on concretions. It states that the high-level module
must not depend on the low-level module, but they should depend on abstractions.

The basic idea behind this principle is that your code should depend on abstractions not specifics.
In other words, instead of wiring classes together, the construction details should be abstracted away 
behind an interface or abstract class.

Example -
Here we have a Notification class that sends a message to a EmailService -

```C#
public class EmailService
{
    public void SendEmail(string message)
    {
        // Sends an email...
    }
}

public class Notification
{
    private EmailService _service;

    public Notification()
    {
        this._service = new EmailService();
    }

    public void Notify(string message)
    {
        this._service.SendEmail(message);
    }
}
```

Notification is the high-level module and EmailService is the low-level module.  
The problem here is that Notification is directly dependent on EmailService. This violates the principle
and makes the Notification class difficult to extend or test.

By implementing an IMessageService interface, we can decouple these classes -

```C#
public interface IMessageService
{
    void SendMessage(string message);
}

public class EmailService : IMessageService
{
    public void SendMessage(string message)
    {
        // Sends an email...
    }
}

public class Notification
{
    private IMessageService _service;

    public Notification(IMessageService svc)
    {
        this._service = svc;
    }

    public void Notify(string message)
    {
        this._service.SendMessage(message);
    }
}
```

Now, the Notification class is not directly dependent on EmailService. It only depends on the IMessageService
abstraction. This makes our Notification class more flexible (it can now easily notify via any service
implementing IMessageService) and easy to test (we can simply pass a mock message service into Notification
for testing).