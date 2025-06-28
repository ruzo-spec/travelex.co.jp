# travelex.co.jp
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>強力パスワード生成ツール</title>
</head>
<body>
  <h1>🔐 あなたのパスワード</h1>
  <button onclick="loadPassword()">パスワードを表示</button>
  <p id="result">ここに表示されます</p>

  <hr>
  <form id="emailForm">
    <input type="email" name="user_email" placeholder="メールアドレス" required />
    <button type="submit">認証コードを送信</button>
  </form>

  <input type="text" id="auth_input" placeholder="認証コードを入力" />
  <button onclick="checkCode()">認証確認</button>

  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@2.6.4/dist/email.min.js"></script>
  <script src="script.js"></script>
</body>
</html>
