<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>CasinoZone</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@600&display=swap');

    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: #1e1e2f;
      color: #fff;
    }

    nav {
      background: #2c2c3e;
      padding: 10px;
      display: flex;
      justify-content: space-around;
      border-bottom: 2px solid #ffcc00;
    }

    nav a {
      color: #ffcc00;
      text-decoration: none;
      font-weight: bold;
    }

    .balance {
      text-align: right;
      padding: 10px 15px;
      font-size: 1.1rem;
      font-family: 'Orbitron', sans-serif;
      color: #00ffcc;
      background: #2c2c3e;
    }

    .container {
      padding: 20px;
    }

    .section {
      display: none;
    }

    .active {
      display: block;
    }

    .slot-machine {
      display: flex;
      justify-content: center;
      margin: 20px 0;
    }

    .reel {
      width: 60px;
      height: 60px;
      background: #444;
      font-size: 2rem;
      margin: 0 5px;
      display: flex;
      align-items: center;
      justify-content: center;
      border-radius: 10px;
    }

    .board {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 5px;
      max-width: 200px;
      margin: 20px auto;
    }

    .cell {
      background: #333;
      width: 60px;
      height: 60px;
      font-size: 2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      border-radius: 5px;
    }

    button {
      padding: 10px 20px;
      background: #ffcc00;
      border: none;
      color: #1e1e2f;
      font-weight: bold;
      border-radius: 10px;
      cursor: pointer;
      margin: 10px 0;
    }

    .form, .bet-input {
      max-width: 300px;
      margin: auto;
      background: #2b2b40;
      padding: 20px;
      border-radius: 10px;
    }

    .form input, .form select, .bet-input input {
      width: 100%;
      margin-bottom: 10px;
      padding: 10px;
      border-radius: 5px;
      border: none;
    }

    footer {
      text-align: center;
      font-size: 0.8em;
      margin-top: 40px;
      color: #888;
    }
  </style>
</head>
<body>

  <div class="balance">Saldo: <span id="balance">Rp100.000</span></div>

  <nav>
    <a href="#" onclick="showSection('slot')">Slot</a>
    <a href="#" onclick="showSection('tic-tac-toe')">Tic Tac Toe</a>
    <a href="#" onclick="showSection('deposit')">Deposit</a>
    <a href="#" onclick="showSection('withdraw')">Withdraw</a>
  </nav>

  <div class="container">

    <!-- SLOT -->
    <div class="section active" id="slot">
      <h2>Slot Emoji</h2>
      <div class="bet-input">
        <label>Taruhan (min 50.000):</label>
        <input type="number" id="slotBet" value="50000" />
      </div>
      <div class="slot-machine">
        <div class="reel" id="reel1">🍒</div>
        <div class="reel" id="reel2">🍒</div>
        <div class="reel" id="reel3">🍒</div>
      </div>
      <button onclick="spin()">SPIN</button>
      <p id="slot-result"></p>
    </div>

    <!-- TIC TAC TOE -->
    <div class="section" id="tic-tac-toe">
      <h2>Tebak Tic Tac Toe vs AI</h2>
      <div class="bet-input">
        <label>Taruhan (min 50.000):</label>
        <input type="number" id="tttBet" value="50000" />
      </div>
      <div class="board" id="board"></div>
      <p id="ttt-status"></p>
    </div>

    <!-- DEPOSIT -->
    <div class="section" id="deposit">
      <h2>Deposit</h2>
      <div class="form">
        <input type="number" placeholder="Jumlah (Rp)" id="depositAmount"/>
        <select>
          <option>Bank Transfer</option>
          <option>PayPal</option>
          <option>Crypto (BTC/ETH/USDT)</option>
          <option>Gopay</option>
          <option>OVO</option>
          <option>DANA</option>
          <option>AliPay</option>
          <option>Wise</option>
        </select>
        <button onclick="deposit()">Deposit</button>
      </div>
    </div>

    <!-- WITHDRAW -->
    <div class="section" id="withdraw">
      <h2>Withdraw</h2>
      <div class="form">
        <input type="number" placeholder="Jumlah (Rp)" id="withdrawAmount"/>
        <select>
          <option>Bank Transfer</option>
          <option>PayPal</option>
          <option>Crypto (BTC/ETH/USDT)</option>
          <option>Gopay</option>
          <option>OVO</option>
          <option>DANA</option>
          <option>AliPay</option>
          <option>Wise</option>
        </select>
        <button onclick="withdraw()">Withdraw</button>
      </div>
    </div>
  </div>

  <footer>CasinoZone &copy; 2025 – All Rights Reserved</footer>

  <script>
    let balance = 100000;
    const emojis = ['7️⃣', '🍫', '🍒', '💰', '🃏'];

    function formatRupiah(number) {
      return 'Rp' + number.toLocaleString('id-ID');
    }

    function updateBalanceDisplay() {
      document.getElementById('balance').innerText = formatRupiah(balance);
    }

    function showSection(id) {
      document.querySelectorAll('.section').forEach(sec => sec.classList.remove('active'));
      document.getElementById(id).classList.add('active');
    }

    // SLOT
    function spin() {
      const bet = parseInt(document.getElementById('slotBet').value);
      if (bet > balance || bet < 50000) {
        alert('Minimal taruhan Rp50.000 dan saldo harus cukup!');
        return;
      }

      const r1 = emojis[Math.floor(Math.random() * emojis.length)];
      const r2 = emojis[Math.floor(Math.random() * emojis.length)];
      const r3 = emojis[Math.floor(Math.random() * emojis.length)];

      document.getElementById('reel1').innerText = r1;
      document.getElementById('reel2').innerText = r2;
      document.getElementById('reel3').innerText = r3;

      if (r1 === r2 && r2 === r3) {
        balance += bet * 5;
        document.getElementById('slot-result').innerText = `JACKPOT! Menang ${formatRupiah(bet * 5)}`;
      } else {
        balance -= bet;
        document.getElementById('slot-result').innerText = `Kalah ${formatRupiah(bet)}`;
      }

      updateBalanceDisplay();
    }

    // TIC TAC TOE
    let board = Array(9).fill(null);

    function renderBoard() {
      const boardEl = document.getElementById('board');
      boardEl.innerHTML = '';
      board.forEach((val, idx) => {
        const cell = document.createElement('div');
        cell.className = 'cell';
        cell.innerText = val || '';
        cell.onclick = () => playerMove(idx);
        boardEl.appendChild(cell);
      });
    }

    function playerMove(index) {
      const bet = parseInt(document.getElementById('tttBet').value);
      if (bet > balance || bet < 50000) {
        alert('Minimal taruhan Rp50.000 dan saldo harus cukup!');
        return;
      }

      if (!board[index]) {
        board[index] = 'X';
        renderBoard();
        if (!checkWinner('X')) aiMove(bet);
      }
    }

    function aiMove(bet) {
      let empty = board.map((v, i) => v === null ? i : null).filter(v => v !== null);
      if (empty.length === 0) return;

      let move;
      if (bet >= 100000) {
        for (let i of empty) {
          board[i] = 'O';
          if (checkWinner('O', false)) {
            move = i;
            break;
          }
          board[i] = null;
        }
      }
      if (move === undefined) move = empty[Math.floor(Math.random() * empty.length)];
      board[move] = 'O';
      renderBoard();
      checkWinner('O');
    }

    function checkWinner(player, show = true) {
      const wins = [
        [0,1,2], [3,4,5], [6,7,8],
        [0,3,6], [1,4,7], [2,5,8],
        [0,4,8], [2,4,6]
      ];
      for (let combo of wins) {
        const [a,b,c] = combo;
        if (board[a] === player && board[b] === player && board[c] === player) {
          if (show) {
            const bet = parseInt(document.getElementById('tttBet').value);
            if (player === 'X') {
              balance += bet * 2;
              document.getElementById('ttt-status').innerText = `Kamu Menang! +${formatRupiah(bet * 2)}`;
            } else {
              balance -= bet;
              document.getElementById('ttt-status').innerText = `Kamu Kalah! -${formatRupiah(bet)}`;
            }
            updateBalanceDisplay();
            setTimeout(resetTicTacToe, 1500);
          }
          return true;
        }
      }
      if (!board.includes(null)) {
        document.getElementById('ttt-status').innerText = 'Seri!';
        setTimeout(resetTicTacToe, 1500);
        return true;
      }
      return false;
    }

    function resetTicTacToe() {
      board = Array(9).fill(null);
      document.getElementById('ttt-status').innerText = '';
      renderBoard();
    }

    // Deposit & Withdraw
    function deposit() {
      const amount = parseInt(document.getElementById('depositAmount').value);
      if (amount > 0) {
        balance += amount;
        updateBalanceDisplay();
        alert('Deposit berhasil!');
      }
    }

    function withdraw() {
      const amount = parseInt(document.getElementById('withdrawAmount').value);
      if (amount > 0 && amount <= balance) {
        balance -= amount;
        updateBalanceDisplay();
        alert('Withdraw berhasil!');
      } else {
        alert('Saldo tidak cukup!');
      }
    }

    // Init
    renderBoard();
    updateBalanceDisplay();
  </script>
</body>
</html>
