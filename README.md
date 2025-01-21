<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Personal Budget Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            padding: 0;
            background-color: #f4f4f9;
            color: #333;
        }
        h1, h2 {
            text-align: center;
            color: #555;
        }
        .container {
            max-width: 600px;
            margin: auto;
            padding: 20px;
            background: #fff;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        .button {
            display: block;
            width: 100%;
            padding: 10px;
            margin-top: 10px;
            background: #007BFF;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            text-align: center;
        }
        .button:hover {
            background: #0056b3;
        }
        input, select {
            width: calc(100% - 20px);
            margin: 10px 0;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        table, th, td {
            border: 1px solid #ddd;
        }
        th, td {
            padding: 10px;
            text-align: left;
        }
        th {
            background-color: #f4f4f4;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Personal Budget Tracker</h1>
        <div>
            <h2>Add Transaction</h2>
            <label for="category">Category:</label>
            <input type="text" id="category" placeholder="e.g., Food, Rent, Utilities">
            <label for="type">Type:</label>
            <select id="type">
                <option value="Income">Income</option>
                <option value="Expense">Expense</option>
            </select>
            <label for="amount">Amount:</label>
            <input type="number" id="amount" placeholder="Enter amount (RM)">
            <button class="button" onclick="addTransaction()">Add Transaction</button>
        </div>

        <div>
            <h2>Transactions</h2>
            <table id="transactions-table">
                <thead>
                    <tr>
                        <th>Category</th>
                        <th>Type</th>
                        <th>Amount (RM)</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Transactions will be displayed here -->
                </tbody>
            </table>
        </div>

        <div>
            <h2>Summary</h2>
            <p id="summary"></p>
        </div>
    </div>

    <script>
        const transactions = [];

        function addTransaction() {
            const category = document.getElementById('category').value;
            const type = document.getElementById('type').value;
            const amountInput = document.getElementById('amount').value;

            if (!category || !amountInput) {
                alert("Please enter all fields.");
                return;
            }

            const amount = parseFloat(amountInput);
            const transaction = { category, type, amount: type === 'Expense' ? -amount : amount };
            transactions.push(transaction);
            displayTransactions();
            generateSummary();
            clearInputs();
        }

        function displayTransactions() {
            const tableBody = document.getElementById('transactions-table').querySelector('tbody');
            tableBody.innerHTML = '';
            transactions.forEach((transaction) => {
                const row = `<tr>
                    <td>${transaction.category}</td>
                    <td>${transaction.type}</td>
                    <td>${transaction.amount.toFixed(2)}</td>
                </tr>`;
                tableBody.innerHTML += row;
            });
        }

        function generateSummary() {
            let totalIncome = 0;
            let totalExpense = 0;

            transactions.forEach((transaction) => {
                if (transaction.type === 'Income') {
                    totalIncome += transaction.amount;
                } else if (transaction.type === 'Expense') {
                    totalExpense += transaction.amount;
                }
            });

            const netBalance = totalIncome + totalExpense;
            document.getElementById('summary').innerHTML = `
                <strong>Total Income:</strong> RM${totalIncome.toFixed(2)}<br>
                <strong>Total Expenses:</strong> RM${(-totalExpense).toFixed(2)}<br>
                <strong>Net Balance:</strong> RM${netBalance.toFixed(2)}
            `;
        }

        function clearInputs() {
            document.getElementById('category').value = '';
            document.getElementById('amount').value = '';
        }
    </script>
</body>
</html>
