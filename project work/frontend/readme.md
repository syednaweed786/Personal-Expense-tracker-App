*Frontend (React.js):*

1. *App.js:*

javascript
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function App() {
    const [expenses, setExpenses] = useState([]);
    const [newExpense, setNewExpense] = useState({
        description: '',
        amount: 0,
        date: new Date().toISOString().slice(0, 10)
    });

    useEffect(() => {
        fetchExpenses();
    }, []);

    const fetchExpenses = async () => {
        try {
            const response = await axios.get('/api/expenses');
            setExpenses(response.data);
        } catch (err) {
            console.error(err);
        }
    };

    const addExpense = async () => {
        try {
            const response = await axios.post('/api/expenses', newExpense);
            setExpenses([...expenses, response.data]);
            setNewExpense({ description: '', amount: 0, date: new Date().toISOString().slice(0, 10) });
        } catch (err) {
            console.error(err);
        }
    };

    const deleteExpense = async (id) => {
        try {
            await axios.delete(`/api/expenses/${id}`);
            setExpenses(expenses.filter(expense => expense._id !== id));
        } catch (err) {
            console.error(err);
        }
    };

    return (
        <div>
            <h1>Expense Tracker</h1>
            <input
                type="text"
                placeholder="Description"
                value={newExpense.description}
                onChange={e => setNewExpense({ ...newExpense, description: e.target.value })}
            />
            <input
                type="number"
                placeholder="Amount"
                value={newExpense.amount}
                onChange={e => setNewExpense({ ...newExpense, amount: e.target.value })}
            />
            <input
                type="date"
                value={newExpense.date}
                onChange={e => setNewExpense({ ...newExpense, date: e.target.value })}
            />
            <button onClick={addExpense}>Add Expense</button>
            <ul>
                {expenses.map(expense => (
                    <li key={expense._id}>
                        <span>{expense.description} - ${expense.amount} - {new Date(expense.date).toLocaleDateString()}</span>
                        <button onClick={() => deleteExpense(expense._id)}>Delete</button>
                    </li>
                ))}
            </ul>
        </div>
    );
}

export default App;
