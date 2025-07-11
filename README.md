# vkusnaya-şaurma
DOCTYPE html><html lang="ru">
<head>
  <meta charset="UTF-8" />
  <title>Вкусная Шаурма</title>
  <script src="https://unpkg.com/html5-qrcode@2.2.7/minified/html5-qrcode.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
  <style>
    body {
      font-family: sans-serif;
      margin: 0;
      padding: 0;
      background-color: #fff;
      color: #222;
    }
    header {
      background-color: #FFD700;
      color: #000;
      padding: 20px;
      text-align: center;
    }
    nav {
      background: #fff;
      padding: 10px;
      text-align: center;
    }
    nav a {
      margin: 0 10px;
      color: #FFD700;
      text-decoration: none;
      font-weight: bold;
    }
    .menu {
      display: flex;
      justify-content: center;
      gap: 20px;
      flex-wrap: wrap;
      padding: 20px;
    }
    .item {
      background: #fff9c4;
      border-radius: 12px;
      padding: 15px;
      width: 200px;
      text-align: center;
    }
    .item img {
      width: 100%;
      border-radius: 10px;
    }
    .qr-section, .bonus-section, .contact-section {
      padding: 20px;
      text-align: center;
      background-color: #fff9e6;
      margin: 20px;
      border-radius: 10px;
    }
    #qr-reader {
      width: 300px;
      margin: auto;
    }
    .btn {
      background-color: #FFD700;
      color: #000;
      padding: 10px 15px;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      text-decoration: none;
      display: inline-block;
      margin-top: 10px;
    }
  </style>
</head>
<body>  <header>
    <h1>Вкусная Шаурма</h1>
    <p>Горячая, свежая, вкусная!</p>
    <img src="https://via.placeholder.com/150x100.png?text=%D0%9B%D0%BE%D0%B3%D0%BE+%D1%81+%D1%88%D0%B0%D1%83%D1%80%D0%BC%D0%B0" alt="Логотип">
  </header>  <nav>
    <a href="#menu">Меню</a>
    <a href="#bonus">Бонусы</a>
    <a href="#qr">QR-Сканер</a>
    <a href="#contact">Связаться</a>
  </nav>  <section id="menu" class="menu">
    <div class="item">
      <img src="https://via.placeholder.com/200x130?text=%D0%A8%D0%B0%D1%83%D1%80%D0%BC%D0%B0+%D1%81+%D0%BC%D1%8F%D1%81%D0%BE%D0%BC" alt="Мясная">
      <h3>Шаурма с мясом</h3>
      <p>Классическая говядина</p>
    </div>
    <div class="item">
      <img src="https://via.placeholder.com/200x130?text=%D0%A1+%D0%BA%D1%83%D1%80%D0%B8%D1%86%D0%B5%D0%B9" alt="Куриная">
      <h3>Шаурма с курицей</h3>
      <p>Нежная курочка</p>
    </div>
    <div class="item">
      <img src="https://via.placeholder.com/200x130?text=%D0%92%D0%B5%D0%B3%D0%B5-%D1%88%D0%B0%D1%83%D1%80%D0%BC%D0%B0" alt="Вегетарианская">
      <h3>Вегетарианская</h3>
      <p>Без мяса, с любовью</p>
    </div>
  </section>  <section id="bonus" class="bonus-section">
    <h2>🏱 Бонусная система</h2>
    <input type="text" id="phone" placeholder="Введите номер телефона" />
    <button class="btn" onclick="getBonus()">Проверить бонус</button>
    <p id="bonus-display">Ваш бонусный счёт: --</p>
  </section>  <section id="qr" class="qr-section">
    <h2>📷 Отсканируйте QR-код</h2>
    <div id="qr-reader"></div>
    <p id="qr-result">Ожидание сканирования...</p>
  </section>  <section id="contact" class="contact-section">
    <h2>📞 Связь и заказ</h2>
    <p>Позвоните нам или пишите в WhatsApp:</p>
    <p><strong>+90 555 111 2233</strong></p>
    <a class="btn" href="https://wa.me/905551112233">Заказать через WhatsApp</a>
  </section>  <script>
    function getBonus() {
      const phone = document.getElementById("phone").value;
      if (!phone) return alert("Введите номер телефона!");

      axios.get(`http://localhost:3000/bonus/${phone}`)
        .then(res => {
          document.getElementById("bonus-display").innerText = `Ваш бонусный счёт: ${res.data.bonus} баллов`;
        })
        .catch(err => {
          alert("Ошибка при получении бонусов");
        });
    }

    function onScanSuccess(decodedText) {
      document.getElementById("qr-result").innerText = `Сканировано: ${decodedText}`;
      const phone = document.getElementById("phone").value;
      if (!phone) return alert("Сначала введите номер телефона!");

      axios.post(`http://localhost:3000/bonus/${phone}/add`, { amount: 5 })
        .then(res => {
          alert("5 бонусов добавлено!");
          getBonus();
        })
        .catch(err => alert("Ошибка начисления бонуса"));
    }

    const qrScanner = new Html5Qrcode("qr-reader");
    qrScanner.start(
      { facingMode: "environment" },
      { fps: 10, qrbox: 250 },
      onScanSuccess
    ).catch(err => {
      console.error("Ошибка сканера:", err);
    });
  </script></body>
</html>
