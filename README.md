<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>パスワード生成</title>
  <style>
    /* style.css の内容 */
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: #f9f9f9;
      color: #333;
      transition: background 0.3s;
    }
    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem 2rem;
      background-color: #ffffff;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
      position: sticky;
      top: 0;
      z-index: 10;
    }
    header h1 {
      margin: 0;
      color: #3a3a3a;
    }
    nav a {
      margin-left: 1rem;
      text-decoration: none;
      color: #0077cc;
      font-weight: bold;
      cursor: pointer;
    }
    main {
      padding: 2rem;
      max-width: 600px;
      margin: auto;
    }
    .step {
      display: none;
      opacity: 0;
      transform: translateX(50px);
      transition: all 0.5s ease;
    }
    .step.active {
      display: block;
      opacity: 1;
      transform: translateX(0);
    }
    input, textarea, button {
      display: block;
      width: 100%;
      padding: 0.75rem;
      margin: 1rem 0;
      border: 1px solid #ccc;
      border-radius: 6px;
      font-size: 1rem;
    }
    button {
      background-color: #0077cc;
      color: white;
      cursor: pointer;
      transition: background 0.3s ease;
    }
    button:hover {
      background-color: #005fa3;
    }
    .password-box {
      background-color: #eee;
      padding: 1rem;
      font-size: 1.2rem;
      word-break: break-word;
      text-align: center;
      border-radius: 8px;
      border: 1px dashed #aaa;
    }
    footer {
      background: #fafafa;
      padding: 2rem;
      margin-top: 4rem;
      border-top: 1px solid #ddd;
    }
    #g-recaptcha {
      margin: 1rem 0;
    }
    textarea {
      resize: vertical;
      min-height: 100px;
    }
    @media (max-width: 600px) {
      main {
        padding: 1rem;
      }
      header {
        flex-direction: column;
        align-items: flex-start;
      }
      nav a {
        margin-left: 0;
        margin-top: 0.5rem;
      }
    }
  </style>

  <script src="https://cdn.jsdelivr.net/npm/emailjs-com@2/dist/email.min.js"></script>
  <script src="https://www.google.com/recaptcha/api.js" async defer></script>
</head>
<body>
  <header>
    <h1>SecureGen</h1>
    <nav>
      <a onclick="showStep(0)">Home</a>
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

  <script>
    // ページステップ管理
    let currentStep = 0;
    const steps = document.querySelectorAll('.step');
    function showStep(index) {
      if (index < 0 || index >= steps.length) return;
      steps.forEach((step, i) => {
        step.classList.toggle('active', i === index);
      });
      currentStep = index;
    }

    // EmailJS初期化（YOUR_PUBLIC_KEYに置き換えてください）
    emailjs.init("9HNN-K7grBupR0XAg");

    // 認証コード保持
    let authCode = "";

    // メール送信処理（認証コード送信）
    document.getElementById('emailForm').addEventListener('submit', function(e) {
      e.preventDefault();
      authCode = Math.floor(100000 + Math.random() * 900000).toString();
      const email = e.target.user_email.value;

      emailjs.send("YOUR_SERVICE_ID", "YOUR_TEMPLATE_ID", {
        to_email: email,
        code: authCode
      }).then(() => {
        alert("認証コードを送信しました。メールを確認してください。");
        showStep(1);
      }).catch(err => {
        alert("送信エラー: " + err);
      });
    });

    // reCAPTCHA成功時にボタン有効化
    function onRecaptchaSuccess() {
      document.getElementById('recaptchaBtn').disabled = false;
    }

    // reCAPTCHA「次へ」ボタン処理
    document.getElementById('recaptchaBtn').addEventListener('click', () => {
      showStep(2);
    });

    // 認証コードチェック
    function checkCode() {
      const input = document.getElementById('auth_input').value.trim();
      if (input === authCode) {
        showStep(3);
        loadPassword();
      } else {
        alert("認証失敗。コードが正しくありません。");
      }
    }

    // ランダムパスワード生成関数
    function generateRandomPassword(length = 16) {
      const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789!@#$%^&*()_+{}[]:;<>,.?/-=";
      let password = "";
      for (let i = 0; i < length; i++) {
        password += chars.charAt(Math.floor(Math.random() * chars.length));
      }
      return password;
    }

    // 300個のパスワード生成リスト
    const passwordList = Array.from({ length: 300 }, () => generateRandomPassword());

    // パスワード表示
    function loadPassword() {
      const index = Math.floor(Math.random() * passwordList.length);
      document.getElementById('result').textContent = passwordList[index];
    }

    // お問い合わせ送信処理
    function sendInquiry() {
      const form = document.getElementById('inquiryForm');
      emailjs.sendForm("ruzo685@adadad.uk", "認証を行ってください", form)
        .then(() => alert("お問い合わせを送信しました。"))
        .catch(error => alert("エラーが発生しました: " + error));
    }
  </script>
</body>
</html>
