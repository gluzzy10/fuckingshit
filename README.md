<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Интерактивный тренажер HTML &amp; CSS</title>
  <style>
    :root {
      --bg: #0f172a;
      --card: #1e293b;
      --border: #334155;
      --text: #f8fafc;
      --muted: #94a3b8;
      --accent: #38bdf8;
      --success: #22c55e;
      --error: #ef4444;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: system-ui, -apple-system, sans-serif;
    }

    body {
      background: var(--bg);
      color: var(--text);
      line-height: 1.6;
      padding: 20px;
    }

    .container {
      max-width: 1100px;
      margin: 0 auto;
    }

    header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-bottom: 20px;
      border-bottom: 1px solid var(--border);
      margin-bottom: 24px;
      flex-wrap: wrap;
      gap: 15px;
    }

    h1 {
      font-size: 1.5rem;
      color: var(--accent);
    }

    h2 {
      margin-bottom: 12px;
      font-size: 1.3rem;
    }

    h3 {
      margin: 16px 0 8px;
      font-size: 1.1rem;
      color: var(--accent);
    }

    p {
      margin-bottom: 12px;
      color: #cbd5e1;
    }

    code {
      background: #0f172a;
      padding: 2px 6px;
      border-radius: 4px;
      color: #f472b6;
      font-family: monospace;
    }

    .nav-tabs {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    .tab-btn {
      background: var(--card);
      color: var(--text);
      border: 1px solid var(--border);
      padding: 10px 16px;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 600;
      transition: 0.2s;
    }

    .tab-btn.active {
      border-color: var(--accent);
      background: rgba(56, 189, 248, 0.15);
    }

    .tab-btn.done::after {
      content: " ✓";
      color: var(--success);
    }

    .tab-btn:disabled {
      opacity: 0.5;
      cursor: not-allowed;
    }

    .card {
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 24px;
      margin-bottom: 24px;
    }

    .workspace {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 16px;
      margin-top: 16px;
    }

    @media (max-width: 768px) {
      .workspace {
        grid-template-columns: 1fr;
      }
    }

    .editors {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    label {
      font-size: 0.85rem;
      color: var(--muted);
      font-weight: bold;
      display: block;
      margin-bottom: 4px;
    }

    textarea {
      width: 100%;
      height: 140px;
      background: #090d16;
      color: #e2e8f0;
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 12px;
      font-family: monospace;
      font-size: 14px;
      resize: vertical;
    }

    .preview-box {
      background: #fff;
      border-radius: 8px;
      overflow: hidden;
      display: flex;
      flex-direction: column;
      min-height: 340px;
    }

    .preview-header {
      background: #e2e8f0;
      color: #334155;
      padding: 6px 12px;
      font-size: 0.8rem;
      font-weight: bold;
    }

    iframe {
      width: 100%;
      flex: 1;
      min-height: 300px;
      border: none;
      background: #fff;
    }

    .btn {
      background: var(--accent);
      color: #0f172a;
      border: none;
      padding: 10px 20px;
      border-radius: 8px;
      font-weight: bold;
      cursor: pointer;
      margin-top: 12px;
      transition: 0.2s;
    }

    .btn:hover {
      opacity: 0.9;
    }

    .btn:focus-visible,
    .tab-btn:focus-visible {
      outline: 3px solid #fff;
      outline-offset: 2px;
    }

    .btn-success {
      background: var(--success);
      color: #fff;
    }

    .status-msg {
      margin-top: 10px;
      font-weight: 600;
      font-size: 0.95rem;
    }

    .status-ok {
      color: var(--success);
    }

    .status-err {
      color: var(--error);
    }

    .quiz-item {
      margin-bottom: 18px;
      padding-bottom: 14px;
      border-bottom: 1px solid var(--border);
    }

    .quiz-item p {
      font-weight: 600;
      color: var(--text);
      margin-bottom: 8px;
    }

    .quiz-option {
      display: block;
      margin-bottom: 6px;
      cursor: pointer;
      color: #cbd5e1;
    }

    .quiz-option input {
      margin-right: 8px;
    }

    .cert-controls {
      display: flex;
      gap: 12px;
      margin-bottom: 20px;
      flex-wrap: wrap;
      align-items: center;
    }

    input[type="text"] {
      padding: 10px 14px;
      border-radius: 8px;
      border: 1px solid var(--border);
      background: #0f172a;
      color: #fff;
      font-size: 1rem;
      min-width: min(260px, 100%);
    }

    canvas {
      max-width: 100%;
      border-radius: 8px;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
      display: block;
      margin-top: 16px;
    }

    .hidden {
      display: none !important;
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div>
        <h1>HTML &amp; CSS Тренажер</h1>
        <span style="font-size: 0.85rem; color: var(--muted);">
          Пройдите 3 блока и получите именной сертификат
        </span>
      </div>

      <nav class="nav-tabs" id="navTabs" aria-label="Разделы курса">
        <button class="tab-btn active" type="button" data-tab="0">Блок 1: HTML</button>
        <button class="tab-btn" type="button" data-tab="1">Блок 2: Flexbox</button>
        <button class="tab-btn" type="button" data-tab="2">Блок 3: CSS Grid</button>
        <button class="tab-btn" type="button" data-tab="cert" id="certTabBtn" disabled>
          🏆 Сертификат
        </button>
      </nav>
    </header>

    <main id="moduleContainer"></main>

    <section id="certSection" class="card hidden" aria-labelledby="certTitle">
      <h2 id="certTitle">Поздравляем! Вы успешно завершили курс</h2>
      <p>
        Введите ваше имя и фамилию, чтобы сгенерировать сертификат об окончании
        программы «Основы веб-верстки на HTML и CSS».
      </p>

      <div class="cert-controls">
        <input
          type="text"
          id="studentName"
          maxlength="60"
          autocomplete="name"
          placeholder="Например: Алексей Смирнов"
          aria-label="Имя и фамилия"
        />
        <button class="btn" id="generateBtn" type="button">Сформировать сертификат</button>
        <button class="btn btn-success hidden" id="downloadBtn" type="button">
          Скачать PNG
        </button>
      </div>

      <canvas
        id="certCanvas"
        width="900"
        height="600"
        class="hidden"
        aria-label="Сгенерированный сертификат"
      ></canvas>
    </section>
  </div>

  <script>
    const courseData = [
      {
        title: "Блок 1: Семантический HTML и структура",
        theory: `Семантические теги помогают браузерам и поисковым системам понимать структуру страницы. Используйте <code>&lt;header&gt;</code> для шапки, <code>&lt;main&gt;</code> для основного контента, <code>&lt;h1&gt;</code> для главного заголовка и <code>&lt;button&gt;</code> для интерактивных кнопок.`,
        taskDesc: `Создайте внутри <code>&lt;header&gt;</code> заголовок <code>&lt;h1&gt;</code> с любым текстом и добавьте кнопку <code>&lt;button&gt;</code> с классом <code>btn</code>.`,
        defaultHtml: `<header>
  <!-- Добавьте h1 и button.btn сюда -->

</header>`,
        defaultCss: `header {
  padding: 20px;
  background: #f1f5f9;
  font-family: sans-serif;
}
.btn {
  padding: 8px 16px;
  background: #0284c7;
  color: white;
  border: none;
  border-radius: 4px;
}`,
        validate(doc) {
          const heading = doc.querySelector("header h1");
          const button = doc.querySelector("header button.btn");

          if (!heading || !heading.textContent.trim()) {
            return "Добавьте непустой тег <h1> внутри <header>.";
          }
          if (!button) {
            return "Добавьте тег <button class='btn'> внутри <header>.";
          }
          return true;
        },
        quiz: [
          {
            q: "Какой тег предназначен для главного заголовка страницы?",
            options: ["<head>", "<h1>", "<title>", "<header>"],
            ans: 1
          },
          {
            q: "Какой атрибут делает поле ввода обязательным для заполнения?",
            options: ["mandatory", "validate", "required", "important"],
            ans: 2
          },
          {
            q: "Какой тег используется для создания нумерованного списка?",
            options: ["<ul>", "<li>", "<ol>", "<dl>"],
            ans: 2
          }
        ]
      },
      {
        title: "Блок 2: Стилизация и Flexbox",
        theory: `<code>Flexbox</code> позволяет гибко выравнивать элементы в строку или колонку. Свойство <code>display: flex</code> включает flex-контейнер, <code>justify-content</code> управляет выравниванием по главной оси, а <code>align-items</code> — по поперечной.`,
        taskDesc: `Сделайте блок <code>.card-list</code> flex-контейнером, задайте расстояние между карточками <code>gap: 16px</code> и выровняйте их по центру горизонтально с помощью <code>justify-content: center</code>.`,
        defaultHtml: `<div class="card-list">
  <div class="item">Карточка 1</div>
  <div class="item">Карточка 2</div>
</div>`,
        defaultCss: `.card-list {
  background: #e2e8f0;
  padding: 20px;
  /* Добавьте display, gap и justify-content */
}
.item {
  background: #3b82f6;
  color: white;
  padding: 20px;
  border-radius: 8px;
}`,
        validate(doc, win) {
          const list = doc.querySelector(".card-list");
          if (!list) return "Не найден элемент .card-list";

          const style = win.getComputedStyle(list);
          if (style.display !== "flex") {
            return "Установите display: flex для .card-list.";
          }
          if (style.justifyContent !== "center") {
            return "Добавьте justify-content: center для .card-list.";
          }
          if (style.columnGap !== "16px" && style.gap !== "16px") {
            return "Задайте gap: 16px для .card-list.";
          }
          return true;
        },
        quiz: [
          {
            q: "Какое свойство выравнивает flex-элементы вдоль главной оси?",
            options: ["align-items", "justify-content", "flex-direction", "align-content"],
            ans: 1
          },
          {
            q: "Что делает свойство box-sizing: border-box?",
            options: [
              "Убирает внешние отступы",
              "Включает padding и border в общую ширину элемента",
              "Делает блок строчным",
              "Добавляет тень"
            ],
            ans: 1
          },
          {
            q: "Какой селектор имеет наибольший вес (специфичность)?",
            options: [".class", "div", "#id", "*"],
            ans: 2
          }
        ]
      },
      {
        title: "Блок 3: Адаптивная сетка CSS Grid",
        theory: `<code>CSS Grid</code> — инструмент для создания двумерных сеток. Свойство <code>display: grid</code> активирует сетку, а <code>grid-template-columns</code> определяет количество и ширину колонок. Например, <code>repeat(3, 1fr)</code> создаст три равные колонки.`,
        taskDesc: `Превратите контейнер <code>.gallery</code> в сетку из 3 колонок с помощью <code>grid-template-columns</code> и задайте отступ <code>gap: 12px</code>.`,
        defaultHtml: `<div class="gallery">
  <div class="box">1</div>
  <div class="box">2</div>
  <div class="box">3</div>
</div>`,
        defaultCss: `.gallery {
  /* Настройте Grid-сетку на 3 колонки и gap: 12px */
}
.box {
  background: #10b981;
  color: white;
  padding: 24px;
  text-align: center;
  font-weight: bold;
  border-radius: 6px;
}`,
        validate(doc, win) {
          const grid = doc.querySelector(".gallery");
          if (!grid) return "Не найден элемент .gallery";

          const style = win.getComputedStyle(grid);
          if (style.display !== "grid") {
            return "Установите display: grid для .gallery.";
          }

          const columns = style.gridTemplateColumns.trim().split(/\s+/).filter(Boolean);
          if (columns.length !== 3) {
            return "В сетке должно быть ровно 3 колонки.";
          }

          if (style.columnGap !== "12px" && style.gap !== "12px") {
            return "Задайте gap: 12px для .gallery.";
          }
          return true;
        },
        quiz: [
          {
            q: "Какая единица измерения в CSS Grid означает долю свободного пространства?",
            options: ["em", "vh", "fr", "%"],
            ans: 2
          },
          {
            q: "Какое правило используется для создания стилей под разные размеры экранов?",
            options: ["@import", "@media", "@keyframes", "@supports"],
            ans: 1
          },
          {
            q: "Как задать отступ одновременно между строками и колонками в Grid?",
            options: ["margin", "padding", "gap", "border-spacing"],
            ans: 2
          }
        ]
      }
    ];

    const STORAGE_KEY = "htmlCssProgress";

    function loadProgress() {
      const emptyProgress = () =>
        courseData.map(() => ({ practice: false, quiz: false }));

      try {
        const saved = JSON.parse(localStorage.getItem(STORAGE_KEY));
        if (!Array.isArray(saved) || saved.length !== courseData.length) {
          return emptyProgress();
        }

        return saved.map(item => ({
          practice: item?.practice === true,
          quiz: item?.quiz === true
        }));
      } catch {
        return emptyProgress();
      }
    }

    let progress = loadProgress();
    let previewReady = Promise.resolve();

    function saveProgress() {
      try {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(progress));
      } catch (error) {
        console.warn("Не удалось сохранить прогресс в localStorage.", error);
      }
      updateNavUI();
    }

    function updateNavUI() {
      progress.forEach((item, index) => {
        const button = document.querySelector(`.tab-btn[data-tab="${index}"]`);
        button.classList.toggle("done", item.practice && item.quiz);
      });

      const allDone = progress.every(item => item.practice && item.quiz);
      document.getElementById("certTabBtn").disabled = !allDone;
    }

    function escapeHTML(value) {
      return String(value).replace(/[&<>"']/g, character => ({
        "&": "&amp;",
        "<": "&lt;",
        ">": "&gt;",
        '"': "&quot;",
        "'": "&#39;"
      })[character]);
    }

    function renderModule(index) {
      const module = courseData[index];
      const container = document.getElementById("moduleContainer");

      document.getElementById("certSection").classList.add("hidden");
      container.classList.remove("hidden");

      const quizMarkup = module.quiz.map((item, questionIndex) => `
        <div class="quiz-item">
          <p>${questionIndex + 1}. ${escapeHTML(item.q)}</p>
          ${item.options.map((option, optionIndex) => `
            <label class="quiz-option">
              <input
                type="radio"
                name="q${questionIndex}"
                value="${optionIndex}"
                required
              />
              ${escapeHTML(option)}
            </label>
          `).join("")}
        </div>
      `).join("");

      container.innerHTML = `
        <section class="card">
          <h2>${module.title}</h2>
          <p>${module.theory}</p>

          <h3>Задание</h3>
          <p>${module.taskDesc}</p>

          <div class="workspace">
            <div class="editors">
              <div>
                <label for="htmlCode">HTML</label>
                <textarea id="htmlCode" spellcheck="false">${escapeHTML(module.defaultHtml)}</textarea>
              </div>

              <div>
                <label for="cssCode">CSS</label>
                <textarea id="cssCode" spellcheck="false">${escapeHTML(module.defaultCss)}</textarea>
              </div>
            </div>

            <div class="preview-box">
              <div class="preview-header">Результат в браузере</div>
              <iframe
                id="previewFrame"
                title="Предпросмотр HTML и CSS"
                sandbox="allow-same-origin"
              ></iframe>
            </div>
          </div>

          <button class="btn" id="checkPracticeBtn" type="button">Проверить практику</button>
          <p
            id="practiceStatus"
            class="status-msg ${progress[index].practice ? "status-ok" : ""}"
            aria-live="polite"
          >${progress[index].practice ? "✓ Практическое задание выполнено!" : ""}</p>

          <h3>Проверочный тест</h3>
          <form id="quizForm">
            ${quizMarkup}
            <button class="btn" type="submit">Сдать тест</button>
          </form>
          <p
            id="quizStatus"
            class="status-msg ${progress[index].quiz ? "status-ok" : ""}"
            aria-live="polite"
          >${progress[index].quiz ? "✓ Тест успешно пройден!" : ""}</p>
        </section>
      `;

      document.getElementById("htmlCode").addEventListener("input", updatePreview);
      document.getElementById("cssCode").addEventListener("input", updatePreview);
      document.getElementById("checkPracticeBtn").addEventListener("click", () => {
        checkPractice(index);
      });
      document.getElementById("quizForm").addEventListener("submit", event => {
        event.preventDefault();
        checkQuiz(index);
      });

      updatePreview();
    }

    function updatePreview() {
      const html = document.getElementById("htmlCode").value;
      const css = document.getElementById("cssCode").value;
      const frame = document.getElementById("previewFrame");

      previewReady = new Promise(resolve => {
        frame.addEventListener("load", resolve, { once: true });
        frame.srcdoc = `<!doctype html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <style>${css}</style>
</head>
<body>${html}</body>
</html>`;
      });
    }

    async function checkPractice(index) {
      await previewReady;

      const frame = document.getElementById("previewFrame");
      const status = document.getElementById("practiceStatus");
      const result = courseData[index].validate(
        frame.contentDocument,
        frame.contentWindow
      );

      if (result === true) {
        status.textContent = "✓ Отлично! Практика засчитана.";
        status.className = "status-msg status-ok";
        progress[index].practice = true;
        saveProgress();
      } else {
        status.textContent = `✗ Ошибка: ${result}`;
        status.className = "status-msg status-err";
      }
    }

    function checkQuiz(index) {
      const module = courseData[index];
      const form = document.getElementById("quizForm");
      const status = document.getElementById("quizStatus");

      if (!form.reportValidity()) return;

      let score = 0;
      module.quiz.forEach((item, questionIndex) => {
        const selected = form.querySelector(`input[name="q${questionIndex}"]:checked`);
        if (selected && Number(selected.value) === item.ans) score++;
      });

      if (score === module.quiz.length) {
        status.textContent = `✓ Супер! Все ${score} ответа верны.`;
        status.className = "status-msg status-ok";
        progress[index].quiz = true;
        saveProgress();
      } else {
        status.textContent =
          `✗ Правильных ответов: ${score} из ${module.quiz.length}. Попробуйте ещё раз.`;
        status.className = "status-msg status-err";
      }
    }

    document.getElementById("navTabs").addEventListener("click", event => {
      const button = event.target.closest(".tab-btn");
      if (!button || button.disabled) return;

      document.querySelectorAll(".tab-btn").forEach(tab => {
        tab.classList.remove("active");
      });
      button.classList.add("active");

      const tab = button.dataset.tab;
      if (tab === "cert") {
        document.getElementById("moduleContainer").classList.add("hidden");
        document.getElementById("certSection").classList.remove("hidden");
      } else {
        renderModule(Number(tab));
      }
    });

    function generateCertificate() {
      if (!progress.every(item => item.practice && item.quiz)) {
        alert("Сначала завершите все блоки курса.");
        return;
      }

      const nameInput = document.getElementById("studentName");
      const name = nameInput.value.trim();

      if (!name) {
        alert("Пожалуйста, введите ваше имя и фамилию.");
        nameInput.focus();
        return;
      }

      const canvas = document.getElementById("certCanvas");
      const context = canvas.getContext("2d");

      const gradient = context.createLinearGradient(0, 0, 900, 600);
      gradient.addColorStop(0, "#0f172a");
      gradient.addColorStop(1, "#1e293b");
      context.fillStyle = gradient;
      context.fillRect(0, 0, 900, 600);

      context.strokeStyle = "#38bdf8";
      context.lineWidth = 6;
      context.strokeRect(25, 25, 850, 550);

      context.strokeStyle = "#94a3b8";
      context.lineWidth = 1;
      context.strokeRect(38, 38, 824, 524);

      context.textAlign = "center";
      context.fillStyle = "#38bdf8";
      context.font = "bold 22px system-ui, sans-serif";
      context.fillText("HTML & CSS ACADEMY", 450, 100);

      context.fillStyle = "#ffffff";
      context.font = "bold 46px system-ui, sans-serif";
      context.fillText("СЕРТИФИКАТ", 450, 175);

      context.fillStyle = "#cbd5e1";
      context.font = "20px system-ui, sans-serif";
      context.fillText("Настоящим подтверждается, что", 450, 235);

      context.fillStyle = "#38bdf8";
      context.font = "bold 40px system-ui, sans-serif";
      context.fillText(name, 450, 300, 760);

      context.fillStyle = "#cbd5e1";
      context.font = "18px system-ui, sans-serif";
      context.fillText(
        "успешно прошёл(ла) все 3 практических блока и тестирования",
        450,
        365
      );

      context.fillStyle = "#ffffff";
      context.font = "bold 22px system-ui, sans-serif";
      context.fillText(
        "«Современная веб-верстка: HTML5, Flexbox и CSS Grid»",
        450,
        405
      );

      const date = new Date().toLocaleDateString("ru-RU");
      const certificateId =
        "ID-" + Math.random().toString(36).slice(2, 8).toUpperCase();

      context.fillStyle = "#94a3b8";
      context.font = "16px monospace";
      context.textAlign = "left";
      context.fillText(`Дата выдачи: ${date}`, 70, 515);

      context.textAlign = "right";
      context.fillText(`Номер сертификата: ${certificateId}`, 830, 515);

      canvas.classList.remove("hidden");
      document.getElementById("downloadBtn").classList.remove("hidden");
    }

    function downloadCertificate() {
      const canvas = document.getElementById("certCanvas");
      const link = document.createElement("a");
      link.download = "certificate-html-css.png";
      link.href = canvas.toDataURL("image/png");
      link.click();
    }

    document.getElementById("generateBtn").addEventListener("click", generateCertificate);
    document.getElementById("downloadBtn").addEventListener("click", downloadCertificate);

    updateNavUI();
    renderModule(0);
  </script>
</body>
</html>
