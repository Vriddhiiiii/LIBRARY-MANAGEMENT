import React, { useState } from "react";
import ReactDOM from "react-dom/client";

// BookItem component
function BookItem({ book, onRemove }) {
  return (
    <div style={styles.bookItem}>
      <span>
        <strong>{book.title}</strong> by {book.author}
      </span>
      <button style={styles.removeBtn} onClick={() => onRemove(book.id)}>
        Remove
      </button>
    </div>
  );
}

// Main Library component
function Library() {
  const [books, setBooks] = useState([
    { id: 1, title: "1984", author: "George Orwell" },
    { id: 2, title: "The Hobbit", author: "J.R.R. Tolkien" },
    { id: 3, title: "Pride and Prejudice", author: "Jane Austen" },
  ]);

  const [searchTerm, setSearchTerm] = useState("");
  const [newTitle, setNewTitle] = useState("");
  const [newAuthor, setNewAuthor] = useState("");

  // Add new book
  const handleAddBook = (e) => {
    e.preventDefault();
    if (!newTitle || !newAuthor) return;

    const newBook = {
      id: Date.now(),
      title: newTitle,
      author: newAuthor,
    };

    setBooks([...books, newBook]);
    setNewTitle("");
    setNewAuthor("");
  };

  // Remove book by id
  const handleRemoveBook = (id) => {
    setBooks(books.filter((book) => book.id !== id));
  };

  // Filter books by search term
  const filteredBooks = books.filter(
    (book) =>
      book.title.toLowerCase().includes(searchTerm.toLowerCase()) ||
      book.author.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div style={styles.container}>
      <h1>Library Management</h1>

      {/* Search Box */}
      <input
        type="text"
        placeholder="Search by title or author..."
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        style={styles.searchBox}
      />

      {/* Add Book Form */}
      <form onSubmit={handleAddBook} style={styles.form}>
        <input
          type="text"
          placeholder="Book Title"
          value={newTitle}
          onChange={(e) => setNewTitle(e.target.value)}
          style={styles.input}
        />
        <input
          type="text"
          placeholder="Author"
          value={newAuthor}
          onChange={(e) => setNewAuthor(e.target.value)}
          style={styles.input}
        />
        <button type="submit" style={styles.addBtn}>
          Add Book
        </button>
      </form>

      {/* Book List */}
      <div style={styles.bookList}>
        {filteredBooks.length > 0 ? (
          filteredBooks.map((book) => (
            <BookItem key={book.id} book={book} onRemove={handleRemoveBook} />
          ))
        ) : (
          <p>No books found.</p>
        )}
      </div>
    </div>
  );
}

// Styles
const styles = {
  container: {
    width: "500px",
    margin: "20px auto",
    padding: "20px",
    border: "1px solid #ccc",
    borderRadius: "8px",
    fontFamily: "Arial, sans-serif",
  },
  searchBox: {
    width: "100%",
    padding: "8px",
    marginBottom: "16px",
    borderRadius: "4px",
    border: "1px solid #ccc",
  },
  form: {
    display: "flex",
    marginBottom: "16px",
  },
  input: {
    flex: 1,
    padding: "8px",
    marginRight: "8px",
    borderRadius: "4px",
    border: "1px solid #ccc",
  },
  addBtn: {
    padding: "8px 12px",
    borderRadius: "4px",
    border: "none",
    backgroundColor: "#28a745",
    color: "#fff",
    cursor: "pointer",
  },
  bookList: {
    marginTop: "16px",
  },
  bookItem: {
    display: "flex",
    justifyContent: "space-between",
    padding: "8px 0",
    borderBottom: "1px solid #eee",
  },
  removeBtn: {
    border: "none",
    backgroundColor: "#dc3545",
    color: "#fff",
    borderRadius: "4px",
    padding: "4px 8px",
    cursor: "pointer",
  },
};

// Render the Library component
const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<Library />);
