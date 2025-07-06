# Problem 7.5 Online Book Reader System

## Questions for Solving This Problem
1. Who will use the system? (user, administrator)
2. What features do users need? (reading, bookmarking)
3. What types of books should be supported? (text, audio)
4. What additional features does an administrator need? (add,delete books)
5. If a new book type is added, which parts of the system should be extended?

## Assumptions
- Users are categorized into regular users (Customer) and administrators (adminUser)
- Users can bookmark and save reading progress
- Only two types of reading formats are supported: text and audio
- Administrators can add or delete books

## Structure
- `Book`: Stores book data
- `User` (abstract class): Parent class for `Customer` and `AdminUser`
- `Reader` (interface): Abstracts reading behavior
- `TextBookReader`, `AudioBookReader`: Implementations of reading mechanisms
- `ReaderFactory`: Creates appropriate `Reader` instances based on book type
- `AdminFunction`: Handles administrator-only functions (add, delete, edite)
- `UserFunction`: Handles user functions (bookmarking)

```java
public class Book {
    private String id, title;
    private BookType type;

    public Book(String id, String title, BookType type) {
        this.id = id;
        this.title = title;
        this.type = type;
    }

    public String getTitle() { return title; }
    public BookType getType() { return type; }
}

public abstract class User {
    private String id, name;
    private UserFunction userFunction;
    public abstract void read(Book book);
}

public class Customer extends User {

    @Override
    public void read(Book book) {
        Reader reader = ReaderFactory.getReader(book.getType());
        reader.open(book);
        reader.readPage();
        reader.close();
        userFunction.saveBookmark(book.getTitle(), 10);
    }
}

public class AdminUser extends User {
    private AdminFunction admin;
    public void addBook(Book book) { admin.addBook(book); }
}

public interface Reader {
    void open(Book book);
    void readPage();
    void close();
}

public class TextBookReader implements Reader {
    public void open(Book book) { }
    public void readPage() { }
    public void close() { }
}

public class AudioBookReader implements Reader {
    public void open(Book book) { }
    public void readPage() { }
    public void close() { }
}

public class ReaderFactory {
    public static Reader getReader(BookType type) {
        return switch (type) {
            case TEXT -> new TextBookReader();
            case AUDIO -> new AudioBookReader();
        };
    }
}

public class UserFunction {
    public void saveBookmark() { }
}

public class AdminFunction {
    public void addBook(Book book) { }
}

```

