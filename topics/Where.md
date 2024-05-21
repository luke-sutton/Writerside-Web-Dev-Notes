# Where

Note:
: Filters a collection based on a predicate

```C#
List<Book> books = new List<Book>
{
    new Book { Title = "Book1", Author = "Author1", Year = 2001 },
    new Book { Title = "Book2", Author = "Author2", Year = 2002 },
    new Book { Title = "Book3", Author = "Author1", Year = 2003 },
};

var olderThan2003 = books.Where(book => book.Year < 2003);
```

The above example will return a list of books that match the criteria of the year being before 2003.  

If no elements match the criteria, an error will not be thrown, the result will just be empty.

```C#
List<Book> books = new List<Book>
{
    new Book { Title = "War and Peace", Author = "Leo Tolstoy", Year = 1869 },
    new Book { Title = "Pride and Prejudice", Author = "Jane Austen", Year = 1813 },
    new Book { Title = "The Great Gatsby", Author = "F. Scott Fitzgerald", Year = 1925 },
};

var filteredBooks = books.Where(book => book.Year < 2003 && book.Author.Length >= 4).ToList();
```

The above example shows 2 conditions being checked for, the result is then converted to a List
(from an iEnumerable).