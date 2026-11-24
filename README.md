# fuckingshit
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Интерактивный тренажер HTML & CSS</title>
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
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: system-ui, -apple-system, sans-serif; }
    body { background: var(--bg); color: var(--text); line-height: 1.6; padding: 20px; }
    .container { max-width: 1100px; margin: 0 auto; }
    header { display: flex; justify-content: space-between; align-items: center; padding-bottom: 20px; border-bottom: 1px solid var(--border); margin-bottom: 24px; flex-wrap: wrap; gap: 15px; }
    h1 { font-size: 1.5rem; color: var(--accent); }
    .nav-tabs { display: flex; gap: 10px; flex-wrap: wrap; }
    .tab-btn {
      background: var(--card); color: var(--text); border: 1px solid var(--border);
      padding: 10px 16px; border-radius: 8px; cursor: pointer; font-weight: 600; transition: 0.2s;
    }
    .tab-btn.active { border-color: var(--accent); background: rgba(56, 189, 248, 0.15); }
    .tab-btn.done::after { content: " ✓"; color: var(--success); }
    .tab-btn:disabled { opacity: 0.5; cursor: not-allowed; }
    .card { background: var(--card); border: 1px solid var(--border); border-radius: 12px; padding: 24px; margin-bottom: 24px; }
    h2 { margin-bottom: 12px; font-size: 1.3rem; }
    h3 { margin: 16px 0 8px; font-size: 1.1rem; color: var(--accent); }
    p { margin-bottom: 12px; color: #cbd5e1; }
    code { background: #0f172a; padding: 2px 6px; border-radius: 4px; color: #f472b6; font-family: monospace; }

    /* Редактор практики */
    .workspace { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-top: 16px; }
    @media (max-width: 768px) { .workspace { grid-template-columns: 1fr; } }
    .editors { display: flex; flex-direction: column; gap: 12px; }
    label { font-size: 0.85rem; color: var(--muted); font-weight: bold; display: block; margin-bottom: 4px; }
    textarea {
      width: 100%; height: 140px; background: #090d16; color: #e2e8f0; border: 1px solid var(--border);
      border-radius: 8px; padding: 12px; font-family: monospace; font-size: 14px; resize: vertical;
    }
    .preview-box { background: #ffffff; border-radius: 8px; overflow: hidden; display: flex; flex-direction: column; }
    .preview-header { background: #e2e8f0; color: #334155; padding: 6px 12px; font-size: 0.8rem; font-weight: bold; }
    iframe { width: 100%; height: 100%; min-height: 300px; border: none; background: #fff; }

    /* Кнопки и статусы */
    .btn {
      background: var(--accent); color: #0f172a; border: none; padding: 10px 20px;
      border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 12px; transition: 0.2s;
    }
    .btn:hover { opacity: 0.9; }
    .btn-success { background: var(--success); color: #fff; }
    .status-msg { margin-top: 10px; font-weight: 600; font-size: 0.95rem; }
    .status-ok { color: var(--success); }
    .status-err { color: var(--error); }

    /* Тесты */
    .quiz-item { margin-bottom: 18px; padding-bottom: 14px; border-bottom: 1px solid var(--border); }
    .quiz-item p { font-weight: 600; color: var(--text); margin-bottom: 8px; }
    .quiz-option { display: block; margin-bottom: 6px; cursor: pointer; color: #cbd5e1; }
    .quiz-option input { margin-right: 8px; }

    /* Сертификат */
    .cert-controls { display: flex; gap: 12px; margin-bottom: 20px; flex-wrap: wrap; }
    input[type="text"] {
      padding: 10px 14px; border-radius: 8px; border: 1px solid var(--border);
      background: #0f172a; color: #fff; font-size: 1rem; min-width: 260px;
    }
    canvas { max-width: 100%; border-radius: 8px; box-shadow: 0 10px 25px rgba(0,0,0,0.5); display: block; margin-top: 16px; }
    .hidden { display: none; }
  </style>
</head>
<body>

<div class="container">
  <header>
    <div>
      <h1>HTML & CSS Тренажер</h1>
      <span style="font-size: 0.85rem; color: var(--muted);">Пройдите 3 блока и получите именной сертификат</span>
    </div>
    <nav class="nav-tabs" id="navTabs">
      <button class="tab-btn active" data-tab="0">Блок 1: HTML</button>
      <button class="tab-btn" data-tab="1">Блок 2: Flexbox</button>
      <button class="tab-btn" data-tab="2">Блок 3: CSS Grid</button>
      <button class="tab-btn" data-tab="cert" id="certTabBtn" disabled>🏆 Сертификат</button>
    </nav>
  </header>

  <!-- Контейнер для учебных блоков -->
  <main id="moduleContainer"></main>

  <!-- Секция сертификата -->
  <section id="certSection" class="card hidden">
    <h2>Поздравляем! Вы успешно завершили курс</h2>
    <p>Введите ваше имя и фамилию, чтобы сгенерировать и скачать сертификат об окончании программы «Основы веб-верстки на HTML и CSS».</p>
    <div class="cert-controls">
      <input type="text" id="studentName" placeholder="Например: Алексей Смирнов" />
      <button class="btn" onclick="generateCertificate()">Сформировать сертификат</button>
      <button class="btn btn-success hidden" id="downloadBtn" onclick="downloadCertificate()">Скачать PNG</button>
    </div>
    <canvas id="certCanvas" width="900" height="600" class="hidden"></canvas>
  </section>
</div>

<script>
  // Данные программы обучения (3 блока)
  const courseData = [
    {
      title: "Блок 1: Семантический HTML и структура",
      theory: `Семантические теги помогают браузерам и поисковым системам понимать структуру страницы. Используйте <code>&lt;header&gt;</code> для шапки, <code>&lt;main&gt;</code> для основного контента, <code>&lt;h1&gt;</code> для главного заголовка и <code>&lt;button&gt;</code> для интерактивных кнопок.`,
      taskDesc: "Практика: Создайте внутри <code>&lt;header&gt;</code> заголовок <code>&lt;h1&gt;</code> с любым текстом и добавьте кнопку <code>&lt;button&gt;</code> с классом <code>btn</code>.",
      defaultHtml: `<header>\n  <!-- Добавьте h1 и button.btn сюда -->\n  \n</header>`,
      defaultCss: `header {\n  padding: 20px;\n  background: #f1f5f9;\n  font-family: sans-serif;\n}\n.btn {\n  padding: 8px 16px;\n  background: #0284c7;\n  color: white;\n  border: none;\n  border-radius: 4px;\n}`,
      validate: (doc) => {
        const h1 = doc.querySelector("header h1");
        const btn = doc.querySelector("header button.btn");
        if (!h1 || h1.textContent.trim() === "") return "Добавьте непустой тег <h1> внутри <header>.";
        if (!btn) return "Добавьте тег <button class='btn'> внутри <header>.";
        return true;
      },
      quiz: [
        { q: "Какой тег предназначен для главного заголовка страницы?", options: ["<head>", "<h1>", "<title>", "<header>"], ans: 1 },
        { q: "Какой атрибут делает поле ввода обязательным для заполнения?", options: ["mandatory", "validate", "required", "important"], ans: 2 },
        { q: "Какой тег используется для создания нумерованного списка?", options: ["<ul>", "<li>", "<ol>", "<dl>"], ans: 2 }
      ]
    },
    {
      title: "Блок 2: Стилизация и Flexbox",
      theory: `Модуль <code>Flexbox</code> позволяет гибко выравнивать элементы в одну строку или колонку. Свойство <code>display: flex</code> включает flex-контейнер, <code>justify-content</code> управляет выравниванием по главной оси, а <code>align-items</code> — по поперечной.`,
      taskDesc: "Практика: Сделайте блок <code>.card-list</code> flex-контейнером, задайте отступ между карточками <code>gap: 16px</code> и выровняйте их по центру горизонтально (<code>justify-content: center</code>).",
      defaultHtml: `<div class="card-list">\n  <div class="item">Карточка 1</div>\n  <div class="item">Карточка 2</div>\n</div>`,
      defaultCss: `.card-list {\n  background: #e2e8f0;\n  padding: 20px;\n  /* Напишите стили flex здесь */\n}\n.item {\n  background: #3b82f6;\n  color: white;\n  padding: 20px;\n  border-radius: 8px;\n}`,
      validate: (doc, win) => {
        const list = doc.querySelector(".card-list");
        if (!list) return "Не найден элемент .card-list";
        const style = win.getComputedStyle(list);
        if (style.display !== "flex") return "Установите display: flex для .card-list.";
        if (style.justifyContent !== "center") return "Добавьте justify-content: center для .card-list.";
        return true;
      },
      quiz: [
        { q: "Какое свойство выравнивает flex-элементы вдоль главной оси?", options: ["align-items", "justify-content", "flex-direction", "align-content"], ans: 1 },
        { q: "Что делает свойство box-sizing: border-box?", options: ["Убирает внешние отступы", "Включает padding и border в общую ширину элемента", "Делает блок строчным", "Добавляет тень"], ans: 1 },
        { q: "Какой селектор имеет наибольший вес (специфичность)?", options: [".class", "div", "#id", "*"], ans: 2 }
      ]
    },
    {
      title: "Блок 3: Адаптивная сетка CSS Grid",
      theory: `<code>CSS Grid</code> — мощный инструмент для создания двумерных сеток. Свойство <code>display: grid</code> активирует сетку, а <code>grid-template-columns</code> определяет количество и ширину колонок (например, <code>repeat(3, 1fr)</code> создаст 3 равные колонки).`,
      taskDesc: "Практика: Превратите контейнер <code>.gallery</code> в сетку (<code>display: grid</code>) из 3 колонок с помощью <code>grid-template-columns</code> и задайте отступ <code>gap: 12px</code>.",
      defaultHtml: `<div class="gallery">\n  <div class="box">1</div>\n  <div class="box">2</div>\n  <div class="box">3</div>\n</div>`,
      defaultCss: `.gallery {\n  /* Настройте Grid-сетку на 3 колонки */\n}\n.box {\n  background: #10b981;\n  color: white;\n  padding: 24px;\n  text-align: center;\n  font-weight: bold;\n  border-radius: 6px;\n}`,
      validate: (doc, win) => {
        const grid = doc.querySelector(".gallery");
        if (!grid) return "Не найден элемент .gallery";
Используйте код с осторожностью.
const style = win.getComputedStyle(grid);
if (style.display !== "grid") return "Установите display: grid для .gallery.";
const cols = style.gridTemplateColumns.split(" ").filter(Boolean);
if (cols.length !== 3) return "В сетке должно быть ровно 3 колонки (используйте grid-template-columns).";
return true;
},
quiz: [
{ q: "Какая единица измерения в CSS Grid означает долю свободного пространства?", options: ["em", "vh", "fr", "%"], ans: 2 },
{ q: "Какое правило используется для создания адаптивных стилей под разные экраны?", options: ["@import", "@media", "@keyframes", "@supports"], ans: 1 },
{ q: "Как задать отступ одновременно между строками и колонками в Grid?", options: ["margin", "padding", "gap", "border-spacing"], ans: 2 }
]
}
];
// Состояние прогресса
let progress = JSON.parse(localStorage.getItem("htmlCssProgress")) || [
{ practice: false, quiz: false },
{ practice: false, quiz: false },
{ practice: false, quiz: false }
];
let currentTab = 0;
function saveProgress() {
localStorage.setItem("htmlCssProgress", JSON.stringify(progress));
updateNavUI();
}
function updateNavUI() {
const btns = document.querySelectorAll(".tab-btn");
progress.forEach((p, idx) => {
if (p.practice && p.quiz) btns[idx].classList.add("done");
});
const allDone = progress.every(p => p.practice && p.quiz);
document.getElementById("certTabBtn").disabled = !allDone;
}
function renderModule(index) {
currentTab = index;
document.getElementById("certSection").classList.add("hidden");
const container = document.getElementById("moduleContainer");
container.classList.remove("hidden");
const mod = courseData[index];
const isPracticeDone = progress[index].practice;
const isQuizDone = progress[index].quiz;
container.innerHTML = `

${mod.title}
${mod.theory}
Задание
${mod.taskDesc}
HTML
${mod.defaultHtml}

CSS
${mod.defaultCss}



Результат в браузере



Проверить практику

${isPracticeDone ? '✓ Практическое задание выполнено!' : ''}

Проверочный тест

${mod.quiz.map((item, qIdx) => `
\({qIdx + 1}. \){item.q}
${item.options.map((opt, oIdx) => `
${opt.replace(/</g, "<").replace(/>/g, ">")}
).join("")} </div> ).join("")}
Сдать тест


${isQuizDone ? '✓ Тест успешно пройден!' : ''}


`;
updatePreview();
}
function updatePreview() {
const html = document.getElementById("htmlCode").value;
const css = document.getElementById("cssCode").value;
const frame = document.getElementById("previewFrame");
frame.srcdoc = <!DOCTYPE html><html><head><style>${css}</style></head><body>${html}</body></html>;
}
function checkPractice(index) {
const frame = document.getElementById("previewFrame");
const doc = frame.contentDocument;
const win = frame.contentWindow;
const statusEl = document.getElementById("practiceStatus");
const result = courseData[index].validate(doc, win);
if (result === true) {
statusEl.textContent = "✓ Отлично! Практика засчитана.";
statusEl.className = "status-msg status-ok";
progress[index].practice = true;
saveProgress();
} else {
statusEl.textContent = "✗ Ошибка: " + result;
statusEl.className = "status-msg status-err";
}
}
function checkQuiz(index) {
const mod = courseData[index];
const form = document.getElementById("quizForm");
const statusEl = document.getElementById("quizStatus");
let score = 0;
mod.quiz.forEach((item, qIdx) => {
const selected = form.querySelector(input[name="q${qIdx}"]:checked);
if (selected && Number(selected.value) === item.ans) score++;
});
if (score === mod.quiz.length) {
statusEl.textContent = ✓ Супер! Вы ответили правильно на все ${score} из ${mod.quiz.length} вопросов.;
statusEl.className = "status-msg status-ok";
progress[index].quiz = true;
saveProgress();
} else {
statusEl.textContent = ✗ Правильных ответов: ${score} из ${mod.quiz.length}. Исправьте ошибки и попробуйте снова.;
statusEl.className = "status-msg status-err";
}
}
// Переключение вкладок
document.getElementById("navTabs").addEventListener("click", (e) => {
if (!e.target.classList.contains("tab-btn") || e.target.disabled) return;
document.querySelectorAll(".tab-btn").forEach(b => b.classList.remove("active"));
e.target.classList.add("active");
const tab = e.target.dataset.tab;
if (tab === "cert") {
document.getElementById("moduleContainer").classList.add("hidden");
document.getElementById("certSection").classList.remove("hidden");
} else {
renderModule(Number(tab));
}
});
// Генерация сертификата через Canvas
function generateCertificate() {
const name = document.getElementById("studentName").value.trim();
if (!name) {
alert("Пожалуйста, введите ваше имя и фамилию.");
return;
}
const canvas = document.getElementById("certCanvas");
const ctx = canvas.getContext("2d");
// Фон
const grad = ctx.createLinearGradient(0, 0, 900, 600);
grad.addColorStop(0, "#0f172a");
grad.addColorStop(1, "#1e293b");
ctx.fillStyle = grad;
ctx.fillRect(0, 0, 900, 600);
// Золотая рамка
ctx.strokeStyle = "#38bdf8";
ctx.lineWidth = 6;
ctx.strokeRect(25, 25, 850, 550);
ctx.strokeStyle = "#94a3b8";
ctx.lineWidth = 1;
ctx.strokeRect(38, 38, 824, 524);
// Текст сертификата
ctx.fillStyle = "#38bdf8";
ctx.font = "bold 22px system-ui, sans-serif";
ctx.textAlign = "center";
ctx.fillText("HTML & CSS ACADEMY", 450, 100);
ctx.fillStyle = "#ffffff";
ctx.font = "bold 46px system-ui, sans-serif";
ctx.fillText("СЕРТИФИКАТ", 450, 175);
ctx.fillStyle = "#cbd5e1";
ctx.font = "20px system-ui, sans-serif";
ctx.fillText("Настоящим подтверждается, что", 450, 235);
// Имя студента
ctx.fillStyle = "#38bdf8";
ctx.font = "bold 40px system-ui, sans-serif";
ctx.fillText(name, 450, 300);
ctx.fillStyle = "#cbd5e1";
ctx.font = "20px system-ui, sans-serif";
ctx.fillText("успешно прошел(ла) все 3 практических блока и тестирования по программе", 450, 365);
ctx.fillStyle = "#ffffff";
ctx.font = "bold 22px system-ui, sans-serif";
ctx.fillText("«Современная веб-верстка: HTML5, Flexbox и CSS Grid»", 450, 405);
// Дата и ID
const dateStr = new Date().toLocaleDateString("ru-RU");
const certId = "ID-" + Math.random().toString(36).substring(2, 8).toUpperCase();
ctx.fillStyle = "#94a3b8";
ctx.font = "16px monospace";
ctx.textAlign = "left";
ctx.fillText(Дата выдачи: ${dateStr}, 70, 515);
ctx.textAlign = "right";
ctx.fillText(Номер сертификата: ${certId}, 830, 515);
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
// Инициализация
updateNavUI();
renderModule(0);
