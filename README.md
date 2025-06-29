<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>パスワード生成</title>
  <link rel="stylesheet" href="style.css" />
  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@2/dist/email.min.js"></script>
  <script src="https://www.google.com/recaptcha/api.js" async defer></script>
</head>
<body>
  <header>
    <h1>SecureGen</h1>
    <nav>
      <a href="#" onclick="showStep(0)">Home</a>
      <a href="#inquiry">お問い合わせ</a>
    </nav>
  </header>

  <main>
    <!-- ステップ 1: メール入力 -->
    <section class="step active">
      <h2>ステップ1: メール入力</h2>
      <p>パスワードを受け取るメールアドレスを入力してください。</p>
      <form id="emailForm">
        <input type="email" name="user_email" placeholder="メールアドレス" required />
        <button type="submit">次へ ▶</button>
      </form>
    </section>

    <!-- ステップ 2: reCAPTCHA -->
    <section class="step">
      <h2>ステップ2: ロボット確認</h2>
      <div class="g-recaptcha" data-sitekey="YOUR_RECAPTCHA_SITE_KEY" data-callback="onRecaptchaSuccess"></div>
      <button id="recaptchaBtn" class="next-btn" disabled>次へ ▶</button>
    </section>

    <!-- ステップ 3: コード入力 -->
    <section class="step">
      <h2>ステップ3: 認証コード入力</h2>
      <input type="text" id="auth_input" placeholder="6桁のコードを入力" />
      <button onclick="checkCode()">認証 ▶</button>
    </section>

    <!-- ステップ 4: パスワード表示 -->
    <section class="step">
      <h2>ステップ4: パスワード表示</h2>
      <p>以下があなたの強力パスワードです：</p>
      <div id="result" class="password-box"></div>
    </section>
  </main>

  <!-- お問い合わせ -->
  <footer>
    <section id="inquiry">
      <h2>お問い合わせ</h2>
      <form id="inquiryForm" onsubmit="event.preventDefault(); sendInquiry();">
        <input type="text" name="name" placeholder="お名前" required />
        <input type="email" name="email" placeholder="メールアドレス" required />
        <textarea name="message" placeholder="メッセージ内容" required></textarea>
        <button type="submit">送信</button>
      </form>
    </section>
  </footer>

  <script src="script.js"></script>
</body>
</html>
