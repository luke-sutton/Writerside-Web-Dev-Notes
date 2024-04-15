# Open-Closed Principle

Principle:
: Objects or entities should be open for extension but closed for modification.

This principle means that the behavior of a software entity can be extended without modifying its source code.

The OCP is essential because it promotes software extensibility and maintainability. By allowing
software entities to be extended without modification, developers can add new functionality without
the risk of breaking existing code. This results in code that is easier to maintain, extend, and reuse.

Example:
```C#
public class InvoiceDao
{
    private Invoice invoice;

    public InvoiceDao(Invoice invoice)
    {
        this.invoice = invoice;
    }

    public void SaveToDb()
    {
        // save to database implementation
    }
}
```

The InvoiceDao class has a single responsibility of saving the invoice to the database. But, suppose
there's a new requirement to save the invoice to a file as well. One way to implement this requirement
would be to modify the existing InvoiceDao class by adding a saveToFile() method. But this violates the
Open-Closed Principle because it modifies the existing code that has already been tested and is live in production.

To follow the OCP, a better solution would be to create an InvoiceDao interface and implement it separately
for database and file saving as shown below:
```C#
public interface IInvoiceDao
{
    void Save(Invoice invoice);
}

public class DatabaseInvoiceDao : IInvoiceDao
{
    public void Save(Invoice invoice)
    {
        // save to database implementation
    }
}

public class FileInvoiceDao : IInvoiceDao
{
    public void Save(Invoice invoice)
    {
        // save to file implementation
    }
}
```

This way, if there's a new requirement to save the invoice to another data store, you can implement a new
InvoiceDao implementation without modifying the existing code. Now the InvoiceDao interface is open for
extension and closed for modification, which follows the OCP.