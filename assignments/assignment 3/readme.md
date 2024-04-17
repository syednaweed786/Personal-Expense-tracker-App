1. Set Up Your Project:*

Create a new directory for your project and navigate into it. Then, initialize a new Node.js project and install necessary dependencies:

bash
mkdir express-mongodb-api
cd express-mongodb-api
npm init -y
npm install express mongoose


*2. Create Your Express Server:*

Create a file named server.js and set up your Express server:

javascript
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(bodyParser.json());

// MongoDB connection
mongoose.connect('mongodb://localhost:27017/booksDB', {
  useNewUrlParser: true,
  useUnifiedTopology: true
}).then(() => {
  console.log('Connected to MongoDB');
}).catch(err => {
  console.error('Error connecting to MongoDB:', err);
});

// Routes
app.get('/', (req, res) => {
  res.send('Welcome to the Book API!');
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});


*3. Define Your MongoDB Schema:*

Create a folder named models and a file named Book.js inside it to define your MongoDB schema:

javascript
const mongoose = require('mongoose');

const bookSchema = new mongoose.Schema({
  title: String,
  author: String,
  genre: String,
  publishedYear: Number
});

module.exports = mongoose.model('Book', bookSchema);


*4. Define Routes for CRUD Operations:*

Create a folder named routes and a file named books.js inside it to define routes for CRUD operations on books:

javascript
const express = require('express');
const router = express.Router();
const Book = require('../models/Book');

// GET all books
router.get('/', async (req, res) => {
  try {
    const books = await Book.find();
    res.json(books);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
});

// GET a specific book
router.get('/:id', getBook, (req, res) => {
  res.json(res.book);
});

// CREATE a new book
router.post('/', async (req, res) => {
  const book = new Book({
    title: req.body.title,
    author: req.body.author,
    genre: req.body.genre,
    publishedYear: req.body.publishedYear
  });

  try {
    const newBook = await book.save();
    res.status(201).json(newBook);
  } catch (err) {
    res.status(400).json({ message: err.message });
  }
});

// UPDATE a book
router.patch('/:id', getBook, async (req, res) => {
  if (req.body.title != null) {
    res.book.title = req.body.title;
  }
  if (req.body.author != null) {
    res.book.author = req.body.author;
  }
  if (req.body.genre != null) {
    res.book.genre = req.body.genre;
  }
  if (req.body.publishedYear != null) {
    res.book.publishedYear = req.body.publishedYear;
  }

  try {
    const updatedBook = await res.book.save();
    res.json(updatedBook);
  } catch (err) {
    res.status(400).json({ message: err.message });
  }
});

// DELETE a book
router.delete('/:id', getBook, async (req, res) => {
  try {
    await res.book.remove();
    res.json({ message: 'Book deleted' });
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
});

// Middleware to get a book by ID
async function getBook(req, res, next) {
  let book;
  try {
    book = await Book.findById(req.params.id);
    if (book == null) {
      return res.status(404).json({ message: 'Book not found' });
    }
  } catch (err) {
    return res.status(500).json({ message: err.message });
  }

  res.book = book;
  next();
}

module.exports = router;


*5. Mount Routes in the Main Server File:*

In server.js, mount the book routes:

javascript
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');
const bookRoutes = require('./routes/books');

const app = express();
const PORT = process.env.PORT || 3000;

app.use(bodyParser.json());

mongoose.connect('mongodb://localhost:27017/booksDB', {
  useNewUrlParser: true,
  useUnifiedTopology: true
}).then(() => {
  console.log('Connected to MongoDB');
}).catch(err => {
  console.error('Error connecting to MongoDB:', err);
});

app.use('/api/books', bookRoutes);

app.get('/', (req, res) => {
  res.send('Welcome to the Book API!');
});

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});


*6. Run Your Application:*

Start your Express server:

bash
node server.js

