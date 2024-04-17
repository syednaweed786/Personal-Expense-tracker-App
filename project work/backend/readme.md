*Backend (Node.js with Express.js and MongoDB):*

1. *server.js:*

javascript
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();
const PORT = process.env.PORT || 5000;

app.use(bodyParser.json());

mongoose.connect('mongodb://localhost:27017/expense-tracker', { useNewUrlParser: true, useUnifiedTopology: true })
    .then(() => console.log('Connected to MongoDB'))
    .catch(err => console.error('Failed to connect to MongoDB:', err));

const Expense = require('./models/Expense');

app.get('/api/expenses', async (req, res) => {
    try {
        const expenses = await Expense.find();
        res.json(expenses);
    } catch (err) {
        res.status(500).json({ message: err.message });
    }
});

app.post('/api/expenses', async (req, res) => {
    const expense = new Expense({
        description: req.body.description,
        amount: req.body.amount,
        date: req.body.date
    });

    try {
        const newExpense = await expense.save();
        res.status(201).json(newExpense);
    } catch (err) {
        res.status(400).json({ message: err.message });
    }
});

app.delete('/api/expenses/:id', async (req, res) => {
    try {
        await Expense.findByIdAndDelete(req.params.id);
        res.json({ message: 'Expense deleted' });
    } catch (err) {
        res.status(404).json({ message: 'Expense not found' });
    }
});

app.listen(PORT, () => console.log(`Server is running on port ${PORT}`));


2. *models/Expense.js:*

javascript
const mongoose = require('mongoose');

const expenseSchema = new mongoose.Schema({
    description: { type: String, required: true },
    amount: { type: Number, required: true },
    date: { type: Date, default: Date.now }
});

module.exports = mongoose.model('Expense', expenseSchema);
