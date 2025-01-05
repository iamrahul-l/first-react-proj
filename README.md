# Book Reviews React Application Documentation

## Overview
The Book Reviews application is a web-based platform that allows users to manage and review their book collection. Built with React, it features a responsive design and modern UI components.

## Project Structure

```
├── public/
│   ├── index.css         # Global styles
│   └── favicon-32x32.png # App icon
├── src/
│   ├── components/       # Reusable UI components
│   ├── pages/           # Page-level components
│   └── index.jsx        # Application entry point
├── index.html           # HTML template
└── package.json         # Project dependencies
```

## Key Components

### 1. App ([`src/components/App.jsx`](src/components/App.jsx))
The root component that handles routing and layout:

```jsx
function App() {
  return (
    <Router>
      <div className="App">
        <Navbar />
        <Routes>
          <Route path="/" element={<Homepage />} />
          <Route path="/about" element={<AboutPage />} />
          {/* Additional routes */}
        </Routes>
        <Footer />
      </div>
    </Router>
  );
}
```

### 2. Books ([`src/components/Books.jsx`](src/components/Books.jsx))
Displays the book collection in a grid layout:

```jsx
const Books = () => {
    // State for storing books
    const [books, setBooks] = useState([]);
    
    // Fetch books from API on component mount
    const fetchBooks = async () => {
        const { data } = await Axios.get('https://first-react-proj-o8de.vercel.app/api/books/');
        setBooks(data);
    };

    // Navigate to individual book page when clicked
    const handleBookClick = (bookId) => {
        navigate(`/book/${bookId}`);
    };

    useEffect(() => {
        fetchBooks();
    }, []);

    return (
        <div className='books-grid'>
            {books.map((book) => (
                <div className='book-card' key={book.id} onClick={() => handleBookClick(book.id)}>
                    {/* Book display logic */}
                </div>
            ))}
        </div>
    );
};
```

### 3. Bookopen ([`src/components/Bookopen.jsx`](src/components/Bookopen.jsx))
Displays detailed information about a single book:

```jsx
const Bookopen = () => {
    // Get book ID from URL parameters
    const { id } = useParams();
    
    // State for book details and notes
    const [book, setBook] = useState(null);
    const [notes, setNotes] = useState([]);

    // Fetch both book details and notes
    useEffect(() => {
        const fetchBookDetails = async () => {
            try {
                const bookResponse = await Axios.get(`/api/books/${id}`);
                const notesResponse = await Axios.get(`/api/books/${id}/notes`);
                setBook(bookResponse.data);
                setNotes(notesResponse.data);
            } catch (error) {
                console.error('Error:', error);
            }
        };

        fetchBookDetails();
    }, [id]);

    return (
        <div className='book-details'>
            {/* Book details and notes display logic */}
        </div>
    );
};
```

### 4. AddBooks ([`src/components/Addbooks.jsx`](src/components/Addbooks.jsx))
Handles book addition with OpenLibrary API integration:

```jsx
const BookForm = () => {
    // States for search and form
    const [searchQuery, setSearchQuery] = useState('');
    const [searchResults, setSearchResults] = useState([]);
    const [formData, setFormData] = useState({
        title: '',
        author: '',
        cover_url: '',
        rating: '',
        notes: '',
        isbn: '',
        description: ''
    });

    // OpenLibrary API search
    const searchBooks = async (e) => {
        e.preventDefault();
        const response = await axios.get(`https://openlibrary.org/search.json?q=${searchQuery}`);
        setSearchResults(response.data.docs.slice(0, 5));
    };

    // Auto-fill form when book selected from search
    const selectBook = (book) => {
        const coverUrl = book.cover_i 
            ? `https://covers.openlibrary.org/b/id/${book.cover_i}-L.jpg`
            : '';
        
        setFormData({
            ...formData,
            title: book.title,
            author: book.author_name?.[0] || '',
            cover_url: coverUrl,
            isbn: book.isbn?.[0] || ''
        });
    };

    return (
        <div className="book-form">
            {/* Form elements */}
        </div>
    );
};
```

## Styling
The application uses custom CSS with responsive design principles defined in [`public/index.css`](public/index.css):

```css
:root {
    --primary-color: #007bff;
    --background-dark-color: #10121A;
    // Other variables
}

/* Responsive grid layout */
.books-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 2rem;
}
```

## Features
1. Book Management
   - Add books with OpenLibrary API integration
   - View book details
   - Delete books
   - Rate books

2. Responsive Design
   - Mobile-first approach
   - Adaptive layouts
   - Hamburger menu for mobile

3. User Interface
   - Clean, modern design
   - Animated transitions
   - Loading states

## Installation & Setup
1. Clone the repository
2. Install dependencies:
```sh
npm install
```
3. Start development server:
```sh
npm run dev
```

## Tech Stack
- React
- React Router
- Axios
- Ant Design
- Material UI Icons