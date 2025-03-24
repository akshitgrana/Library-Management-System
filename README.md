# Library Management System

This is a basic database design for a **Library Management System**, which manages books, authors, borrowers, and loans. The system simulates a library environment where users can check out and return books, track borrowed books, and manage author information. It is built using SQL for the database schema and queries, and can be extended to a Python application for interaction with the database.

## Features
- **Authors Table**: Stores information about authors, including name and birth year.
- **Books Table**: Stores information about books, such as title, genre, publication year, and author.
- **Borrowers Table**: Stores information about library borrowers, such as name and email.
- **Loans Table**: Tracks which borrower has borrowed which book and when, including loan and return dates.

## Setup and Installation

### Prerequisites
To set up this system, you'll need a relational database system like SQLite, MySQL, or PostgreSQL. For simplicity, this demo uses **SQLite**, which doesn't require server setup and can be easily tested locally.

### Steps to Setup

1. **Install SQLite** (if not already installed):
   - Visit [SQLite Downloads](https://www.sqlite.org/download.html) and follow the installation instructions for your system.

2. **Create the Database**:
   - Open a terminal or command prompt and run:
     ```bash
     sqlite3 library.db
     ```
   - This will create a new SQLite database file called `library.db`.

3. **Create Tables**:
   - Copy and paste the following SQL schema into the SQLite shell or your SQL client to create the tables:
     ```sql
     -- Create Authors Table
     CREATE TABLE Authors (
         author_id INTEGER PRIMARY KEY AUTOINCREMENT,
         name TEXT NOT NULL,
         birth_year INTEGER
     );

     -- Create Books Table
     CREATE TABLE Books (
         book_id INTEGER PRIMARY KEY AUTOINCREMENT,
         title TEXT NOT NULL,
         genre TEXT,
         publication_year INTEGER,
         author_id INTEGER,
         FOREIGN KEY (author_id) REFERENCES Authors(author_id)
     );

     -- Create Borrowers Table
     CREATE TABLE Borrowers (
         borrower_id INTEGER PRIMARY KEY AUTOINCREMENT,
         name TEXT NOT NULL,
         email TEXT
     );

     -- Create Loans Table
     CREATE TABLE Loans (
         loan_id INTEGER PRIMARY KEY AUTOINCREMENT,
         book_id INTEGER,
         borrower_id INTEGER,
         loan_date TEXT,
         return_date TEXT,
         FOREIGN KEY (book_id) REFERENCES Books(book_id),
         FOREIGN KEY (borrower_id) REFERENCES Borrowers(borrower_id)
     );
     ```

4. **Insert Sample Data**:
   - Insert sample data into the tables to simulate a real library system:
     ```sql
     -- Insert Authors
     INSERT INTO Authors (name, birth_year) 
     VALUES 
         ('J.K. Rowling', 1965),
         ('George Orwell', 1903),
         ('Harper Lee', 1926);

     -- Insert Books
     INSERT INTO Books (title, genre, publication_year, author_id)
     VALUES 
         ('Harry Potter and the Sorcerer\'s Stone', 'Fantasy', 1997, 1),
         ('1984', 'Dystopian', 1949, 2),
         ('To Kill a Mockingbird', 'Fiction', 1960, 3);

     -- Insert Borrowers
     INSERT INTO Borrowers (name, email) 
     VALUES 
         ('Alice Smith', 'alice@example.com'),
         ('Bob Johnson', 'bob@example.com');

     -- Insert Loans
     INSERT INTO Loans (book_id, borrower_id, loan_date, return_date) 
     VALUES 
         (1, 1, '2025-03-01', '2025-03-15'),
         (2, 2, '2025-03-05', '2025-03-20');
     ```

5. **Run Queries**:
   - You can now run SQL queries to interact with the database. For example, to list all books and their authors:
     ```sql
     SELECT Books.title, Authors.name AS author, Books.publication_year
     FROM Books
     JOIN Authors ON Books.author_id = Authors.author_id;
     ```

## Example Python Integration (Optional)

To interact with the database from a Python application, you can use the `sqlite3` library. Here's an example:

```python
import sqlite3

# Connect to the SQLite database
conn = sqlite3.connect('library.db')
cursor = conn.cursor()

# Query: List all books and their authors
cursor.execute('''
    SELECT Books.title, Authors.name AS author, Books.publication_year
    FROM Books
    JOIN Authors ON Books.author_id = Authors.author_id;
''')

# Print the results
for row in cursor.fetchall():
    print(f"Book: {row[0]}, Author: {row[1]}, Published: {row[2]}")

# Close the connection
conn.close()
