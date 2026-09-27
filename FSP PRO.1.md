```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Budget Planner</title>

    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
        }

        body {
            background: #f4f6f9;
            color: #222;
        }

        header {
            background: #243b55;
            color: white;
            padding: 20px;
            text-align: center;
        }

        header h1 {
            margin-bottom: 8px;
        }

        .container {
            width: 90%;
            max-width: 1200px;
            margin: 25px auto;
        }

        .cards {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 15px;
            margin-bottom: 25px;
        }

        .card {
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
        }

        .card h3 {
            color: #555;
            margin-bottom: 10px;
        }

        .card p {
            font-size: 25px;
            font-weight: bold;
        }

        .income {
            color: #16803c;
        }

        .expense {
            color: #d62828;
        }

        .balance {
            color: #2563eb;
        }

        .saving {
            color: #8b5cf6;
        }

        .forms {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .form-box {
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
        }

        .form-box h2 {
            margin-bottom: 20px;
            color: #243b55;
        }

        input, select {
            width: 100%;
            padding: 12px;
            margin: 8px 0;
            border: 1px solid #ccc;
            border-radius: 6px;
        }

        button {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 6px;
            background: #243b55;
            color: white;
            cursor: pointer;
            font-size: 16px;
            margin-top: 8px;
        }

        button:hover {
            background: #141e30;
        }

        .content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
            margin-top: 25px;
        }

        .box {
            background: white;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
        }

        .box h2 {
            margin-bottom: 15px;
            color: #243b55;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 10px;
            border-bottom: 1px solid #ddd;
            text-align: left;
        }

        th {
            background: #f1f3f5;
        }

        .delete-btn {
            background: #dc3545;
            padding: 6px 10px;
            width: auto;
            margin: 0;
        }

        .delete-btn:hover {
            background: #b02a37;
        }

        .alert {
            padding: 15px;
            margin-top: 20px;
            border-radius: 8px;
            display: none;
            background: #fff3cd;
            color: #856404;
        }

        .goal-progress {
            width: 100%;
            height: 20px;
            background: #ddd;
            border-radius: 10px;
            overflow: hidden;
            margin-top: 10px;
        }

        .progress {
            height: 100%;
            width: 0%;
            background: #8b5cf6;
        }

        footer {
            text-align: center;
            padding: 20px;
            margin-top: 30px;
            background: #243b55;
            color: white;
        }

        @media(max-width: 800px) {
            .cards,
            .forms,
            .content {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>

<body>

<header>
    <h1>💰 Budget Planner</h1>
    <p>Manage your income, expenses and savings</p>
</header>

<div class="container">

    <!-- Summary Cards -->
    <div class="cards">

        <div class="card">
            <h3>Total Income</h3>
            <p class="income">₹<span id="totalIncome">0</span></p>
        </div>

        <div class="card">
            <h3>Total Expenses</h3>
            <p class="expense">₹<span id="totalExpense">0</span></p>
        </div>

        <div class="card">
            <h3>Balance</h3>
            <p class="balance">₹<span id="balance">0</span></p>
        </div>

        <div class="card">
            <h3>Savings Goal</h3>
            <p class="saving">₹<span id="goalAmount">0</span></p>
        </div>

    </div>

    <!-- Forms -->
    <div class="forms">

        <!-- Income -->
        <div class="form-box">
            <h2>➕ Add Income</h2>

            <input type="text" id="incomeSource"
                   placeholder="Income source">

            <input type="number" id="incomeAmount"
                   placeholder="Amount">

            <button onclick="addIncome()">
                Add Income
            </button>
        </div>

        <!-- Expense -->
        <div class="form-box">
            <h2>➖ Add Expense</h2>

            <input type="text" id="expenseName"
                   placeholder="Expense name">

            <input type="number" id="expenseAmount"
                   placeholder="Amount">

            <select id="expenseCategory">
                <option value="Food">Food</option>
                <option value="Transport">Transport</option>
                <option value="Education">Education</option>
                <option value="Shopping">Shopping</option>
                <option value="Bills">Bills</option>
                <option value="Entertainment">Entertainment</option>
                <option value="Other">Other</option>
            </select>

            <button onclick="addExpense()">
                Add Expense
            </button>
        </div>

    </div>

    <!-- Savings Goal -->
    <div class="box" style="margin-top:25px;">

        <h2>🎯 Savings Goal</h2>

        <input type="number"
               id="savingsGoal"
               placeholder="Enter savings goal">

        <button onclick="setGoal()">
            Set Savings Goal
        </button>

        <div class="goal-progress">
            <div class="progress" id="progress"></div>
        </div>

        <p style="margin-top:10px;">
            Progress: <span id="progressText">0%</span>
        </p>

    </div>

    <!-- Alert -->
    <div class="alert" id="alert">
        ⚠️ Warning: Your expenses are higher than your income!
    </div>

    <!-- Tables and Chart -->
    <div class="content">

        <div class="box">

            <h2>📋 Transactions</h2>

            <table>
                <thead>
                    <tr>
                        <th>Type</th>
                        <th>Name</th>
                        <th>Category</th>
                        <th>Amount</th>
                        <th>Action</th>
                    </tr>
                </thead>

                <tbody id="transactionTable">
                </tbody>
            </table>

        </div>

        <div class="box">

            <h2>📊 Expense Distribution
```

