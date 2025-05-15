# -

<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>দৈনিক হিসাব অ্যাপ</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f4f8;
      padding: 20px;
      max-width: 500px;
      margin: auto;
    }
    h1 {
      text-align: center;
      color: #333;
    }
    .balance {
      font-size: 24px;
      text-align: center;
      margin: 20px 0;
    }
    form {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    input, select, button {
      padding: 10px;
      font-size: 16px;
    }
    ul {
      list-style: none;
      padding: 0;
      margin-top: 20px;
    }
    li {
      background: #fff;
      padding: 10px;
      margin-bottom: 5px;
      border-left: 5px solid green;
    }
    .expense {
      border-left-color: red;
    }
  </style>
</head>
<body>
  <h1>দৈনিক হিসাব</h1>
  <div class="balance" id="balance">মোট ব্যালেন্স: 0 টাকা</div>

  <form id="form">
    <input type="text" id="description" placeholder="বর্ণনা" required>
    <input type="number" id="amount" placeholder="টাকার পরিমাণ" required>
    <select id="type">
      <option value="income">আয়</option>
      <option value="expense">খরচ</option>
    </select>
    <button type="submit">যোগ করুন</button>
  </form>

  <ul id="list"></ul>

  <script>
    const form = document.getElementById('form');
    const list = document.getElementById('list');
    const balance = document.getElementById('balance');

    let transactions = JSON.parse(localStorage.getItem('transactions')) || [];

    function updateDOM() {
      list.innerHTML = '';
      let total = 0;

      transactions.forEach(item => {
        const li = document.createElement('li');
        li.className = item.type === 'expense' ? 'expense' : '';
        li.innerText = `${item.description} - ${item.amount} টাকা (${item.type === 'income' ? 'আয়' : 'খরচ'})`;
        list.appendChild(li);

        total += item.type === 'income' ? item.amount : -item.amount;
      });

      balance.innerText = `মোট ব্যালেন্স: ${total} টাকা`;
      localStorage.setItem('transactions', JSON.stringify(transactions));
    }

    form.addEventListener('submit', function (e) {
      e.preventDefault();
      const description = document.getElementById('description').value;
      const amount = parseFloat(document.getElementById('amount').value);
      const type = document.getElementById('type').value;

      transactions.push({ description, amount, type });
      updateDOM();
      form.reset();
    });

    updateDOM();
  </script>
</body>
</html>
