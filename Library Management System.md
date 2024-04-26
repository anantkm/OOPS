# Designing a Library Management System

This article contains the object-oriented design and implementation of a Library Management System (LMS) using Python3.

The LMS is designed to manage library operations, including book inventory, member subscriptions, and book lending processes, using Python3's object-oriented features.

## System Requirements
The LMS supports:
1. **Book Inventory Management:** Add, update, and remove books.
2. **Membership Management:** Manage library member information.
3. **Book Lending:** Handle book checkouts and returns.
4. **Search Functionality:** Enable searching for books.

## Core Use Cases
- **Adding Books:** Librarians can add new books to the system.
- **Registering Members:** Register new library members.
- **Lending Books:** Check out books to members.
- **Returning Books:** Manage the return process.

## Key Classes:
- `Library`: Manages the overall library operations.
- `Book`: Represents a book in the library.
- `Member`: Represents a library member.
- `Loan`: Manages the lending of books to members.

## Python3 Implementation

### Book Class
```python
class Book:
    def __init__(self, title: str, author: str, isbn: str):
        self.title: str = title
        self.author: str = author
        self.isbn: str = isbn
        self.is_available: bool = True

    def set_is_available(self, available: bool):
        self.is_available = available

```
### Member Class
```python
from typing import List
import time

class Member:
    def __init__(self, name: str):
        self.name: str = name
        self.member_id: str = self.generate_member_id()
        self.loans: List['Loan'] = []

    def generate_member_id(self) -> str:
        return "MEM" + str(int(time.time() * 1000))

    def add_loan(self, loan: 'Loan'):
        self.loans.append(loan)

```
### Loan Class
```python
from datetime import datetime, timedelta

class Loan:
    def __init__(self, book: Book, member: Member):
        self.book: Book = book
        self.member: Member = member
        self.issue_date: datetime = datetime.now()
        self.due_date: datetime = self.issue_date + timedelta(days=14)
        book.set_is_available(False)
        member.add_loan(self)

    def return_book(self):
        self.book.set_is_available(True)
        self.member.loans.remove(self)

```
### Library Class
```python
from typing import List, Optional

class Library:
    def __init__(self):
        self.books: List[Book] = []
        self.members: List[Member] = []

    def add_book(self, book: Book):
        self.books.append(book)

    def register_member(self, member: Member):
        self.members.append(member)

    def lend_book(self, isbn: str, member: Member):
        book = next((b for b in self.books if b.isbn == isbn and b.is_available), None)
        if book:
            Loan(book, member)

    def search_books_by_title(self, title: str) -> List[Book]:
        return [book for book in self.books if title.lower() in book.title.lower()]

```

### Example Usage
``` python
def main():
    # Initialize the library
    library = Library()

    # Adding books
    library.add_book(Book("Moby Dick", "Herman Melville", "1234567890123"))
    library.add_book(Book("The Great Gatsby", "F. Scott Fitzgerald", "1234567890124"))

    # Registering a member
    member = Member("John Doe")
    library.register_member(member)

    # Lending a book
    library.lend_book("1234567890123", member)

    # Searching for a book
    books_found = library.search_books_by_title("Moby")
    for book in books_found:
        print(f"Found Book: {book.title} by {book.author}")

    # Returning a book
    if member.loans:
        member.loans[0].return_book()
        print(f"Returned Book: {member.loans[0].book.title}")

if __name__ == "__main__":
    main()

```