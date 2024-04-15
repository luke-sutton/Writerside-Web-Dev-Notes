# Single-Responsibility Principle

Principle :
:  A class should have one and only one reason to change, meaning that a class should have only one job.

Benefits that help to build better software -  
1.Testing – A class with one responsibility will have far fewer test cases.  
2.Lower coupling – Less functionality in a single class will have fewer dependencies.  
3.Organization – Smaller, well-organized classes are easier to search than monolithic ones.  

Example:
```C#
public class Marker 
{
    public string Name { get; set; }
    public string Color { get; set; }
    public int Price { get; set; }

    public Marker(string name, string color, int price)
    {
        this.Name = name;
        this.Color = color;
        this.Price = price;
    }
}
```

```C#
public class Invoice
{
    private Marker Marker { get; set; }
    private int Quantity { get; set; }

    public Invoice(Marker marker, int quantity)
    {
        this.Marker = marker;
        this.Quantity = quantity;
    }

    public int CalculateTotal()
    {
        return Marker.Price * this.Quantity;
    }

    public void PrintInvoice()
    {
        // printing implementation
    }
    
    public void SaveToDb()
    {
        // save to database implementation
    }
}
```

The above Invoice class violates the SRP because it has multiple responsibilities – it is
responsible for calculating the total amount, printing the invoice, and saving the invoice to
the database. As a result, if the calculation logic changes, such as the addition of taxes,
the calculateTotal() method would require modification. Similarly, if the printing or
database-saving implementation changes at any point, the class would need to be changed.
There are several reasons for the class to be modified, which could lead to increased maintenance
costs and complexity.

Here's how you can modify the code to follow the SRP:
```C#
class Invoice {
    private Marker marker;
    private int quantity;

    public Invoice(Marker marker, int quantity) {
        this.marker = marker;
        this.quantity = quantity;
    }

    public int calculateTotal() {
        return marker.price * this.quantity;
    }
}

class InvoiceDao {
    private Invoice invoice;

    public InvoiceDao(Invoice invoice) {
        this.invoice = invoice;
    }

    public void saveToDb() {
        // save to database implementation
    }
}

class InvoicePrinter {
    private Invoice invoice;

    public InvoicePrinter(Invoice invoice) {
        this.invoice = invoice;
    }

    public void printInvoice() {
        // printing implementation
    }
}
```

In this refactored example, we have split the responsibilities of the Invoice class into three
separate classes: Invoice, InvoiceDao, and InvoicePrinter.

The Invoice class is responsible only for calculating the total amount, and the printing and saving
responsibilities have been delegated to separate classes. This makes the code more modular, easier to
understand, and less prone to errors.