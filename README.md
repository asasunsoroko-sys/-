<!doctype html>
<html lang="ru">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Викторина: ВОВ и Афганская война</title>
  <style>
    :root {
      --bg: #f5f7fb; --card: #ffffff; --text: #1b1f24;
      --accent: #2f6fed; --ok: #2ebf78; --bad: #ff4d4f; --muted: #6b7280;
    }
    * { box-sizing: border-box; }
    body {
      margin: 0; font-family: system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
      background: var(--bg); color: var(--text);
    }
    header {
      padding: 24px 16px; text-align: center; background: linear-gradient(180deg,#eef2ff,transparent);
    }
    header h1 { margin: 0 0 8px; font-size: 24px; }
    header p { margin: 0; color: var(--muted); }
    .container { max-width: 920px; margin: 0 auto; padding: 16px; }
    .card {
      background: var(--card); border: 1px solid #e5e7eb; border-radius: 12px;
      padding: 16px; margin-bottom: 16px; box-shadow: 0 2px 10px rgba(0,0,0,0.04);
    }
    .card h2 { margin: 0 0 12px; font-size: 20px; }
    .question {
      padding: 12px; border-radius: 10px; border: 1px solid #eceff4; margin-bottom: 10px; background: #fafbff;
    }
    .question.correct { border-color: rgba(46,191,120,.6); background: #f3fcf7; }
    .question.wrong   { border-color: rgba(255,77,79,.6); background: #fff5f5; }
    .q-title { font-weight: 600; margin-bottom: 8px; }
    .options { display: grid; gap: 8px; }
    label.option {
      display: flex; align-items: flex-start; gap: 8px; padding: 10px; border-radius: 8px;
      border: 1px solid #e5e7eb; cursor: pointer; background: #fff;
    }
    .controls {
      display: flex; gap: 12px; flex-wrap: wrap; margin-top: 12px;
    }
    button {
      appearance: none; border: none; padding: 10px 14px; border-radius: 10px;
      background: var(--accent); color: #fff; font-weight: 600; cursor: pointer;
    }
    button.secondary { background: #e5e7eb; color: #111827; }
    .result {
      margin-top: 12px; padding: 12px; border-radius: 10px; background: #eef2ff; color: #111827;
    }
    footer { text-align: center; color: var(--muted); padding: 24px; }
    .hint { font-size: 13px; color: var(--muted); }
  </style>
</head>
<body>
  <header>
    <h1>Викторина: Великая Отечественная война и Афганская война</h1>
    <p>Выберите ответы и нажмите «Проверить». Видно, сколько правильных и где ошибка.</p>
  </header>

  <main class="container">
    <section id="quiz-ww2" class="card">
      <h2>Часть 1. Великая Отечественная война (20 вопросов)</h2>
      <div id="ww2-questions"></div>
      <div class="controls">
        <button id="check-ww2">Проверить ВОВ</button>
        <button class="secondary" id="reset-ww2">Сбросить ответы</button>
      </div>
      <div id="result-ww2" class="result" hidden></div>
      <p class="hint">Подсказка: выбирай один вариант в каждом вопросе.</p>
    </section>

    <section id="quiz-afg" class="card">
      <h2>Часть 2. Афганская война (10 вопросов)</h2>
      <div id="afg-questions"></div>
      <div class="controls">
        <button id="check-afg">Проверить Афганскую войну</button>
        <button class="secondary" id="reset-afg">Сбросить ответы</button>
      </div>
      <div id="result-afg" class="result" hidden></div>
    </section>
  </main>

  <footer>
    Сделано для 5 класса. Можно разместить на GitHub Pages и поделиться QR‑кодом.
  </footer>

  <script>
    // ---- Данные вопросов ----
    const ww2 = [
      // Первые 10 вопросов (как раньше)
      {q: "Когда началась Великая Отечественная война?", opts: ["7 ноября 1941 г.","22 июня 1941 г.","1 сентября 1939 г.","9 мая 1945 г."], correct: 1},
      {q: "Как называется день окончания войны?", opts: ["День Конституции","День Победы","День Независимости","День Защитника Отечества"], correct: 1},

      {q: "Кто был Верховным Главнокомандующим СССР во время войны?", opts: ["Иосиф Сталин","Владимир Ленин","Юрий Гагарин","Лев Толстой"], correct: 0},
      {q: "В каком городе произошла решающая битва 1942–1943 гг.?", opts: ["Киев","Москва","Минск","Сталинград"], correct: 3},
      {q: "Что означает слово «партизан»?", opts: ["Моряк","Солдат регулярной армии","Военный лётчик","Боец, сражающийся в тылу врага"], correct: 3},
      {q: "Как назывался план нападения Германии на СССР?", opts: ["«Барбаросса»","«Тайфун»","«Молния»","«Оверлорд»"], correct: 0},
      {q: "Какой город был в блокаде почти 900 дней?", opts: ["Ленинград","Москва","Минск","Одесса"], correct: 0},
      {q: "Как назывался главный советский танк войны?", opts: ["Т‑34","КВ‑1","ИС‑2","БТ‑7"], correct: 0},
      {q: "Какой праздник отмечается 22 июня?", opts: ["День памяти и скорби","День Победы","День Конституции","День России"], correct: 0},
      {q: "Как назывался парад Победы в Москве в 1945 году?", opts: ["Красный парад","Парад Победы","Парад мира","Парад героев"], correct: 1},
    ];

    const afg = [
      {q: "В каком году началась Афганская война?", opts: ["1979","1985","1991","1975"], correct: 0},
      {q: "Сколько лет длилась Афганская война?", opts: ["5 лет","10 лет","15 лет","20 лет"], correct: 1},
      {q: "В каком году советские войска покинули Афганистан?", opts: ["1989","1991","1985","1993"], correct: 0},
      {q: "Как назывался ввод советских войск в Афганистан?", opts: ["«Операция Багратион»","«Ограниченный контингент»","«Барбаросса»","«Шторм»"], correct: 1},
      {q: "Как называли афганских повстанцев?", opts: ["Моджахеды","Партизаны","Красные гвардейцы","Легионы"], correct: 0},
      {q: "Какой город был столицей Афганистана во время войны?", opts: ["Кабул","Кандагар","Герат","Мазари‑Шариф"], correct: 0},
      {q: "Как называли советских солдат, прошедших Афганистан?", opts: ["Афганцы","Ветераны Победы","Сибиряки","Союзники"], correct: 0},
      {q: "Какой день связан с памятью воинов‑интернационалистов?", opts: ["15 февраля","9 мая","22 июня","23 февраля"], correct: 0},
      {q: "Какой вид войск особенно активно использовался в горах Афганистана?", opts: ["Танковые войска","Воздушно‑десантные войска","Морской флот","Артиллерия"], correct: 1},
      {q: "Как назывался бронетранспортёр, широко применявшийся в Афганистане?", opts: ["БТР‑70","Т‑34","Ил‑2","Як‑9"], correct: 0},
    ];

// ---- Рендер и логика ----
    function renderQuiz(list, mountId, groupName) {
      const mount = document.getElementById(mountId);
      mount.innerHTML = "";
      list.forEach((it, idx) => {
        const qId = ${groupName}-${idx};
        const wrap = document.createElement("div");
        wrap.className = "question";
        wrap.innerHTML = 
          <div class="q-title">${idx+1}. ${it.q}</div>
          <div class="options">
            ${it.opts.map((opt, i) => 
              <label class="option">
                <input type="radio" name="${qId}" value="${i}">
                <span>${String.fromCharCode(97+i)}) ${opt}</span>
              </label>
            ).join("")}
          </div>
        ;
        mount.appendChild(wrap);
      });
    }

    function checkQuiz(list, mountId, resultId, groupName) {
      const mount = document.getElementById(mountId);
      const result = document.getElementById(resultId);
      let correct = 0, answered = 0;

      [...mount.children].forEach((wrap, idx) => {
        const name = ${groupName}-${idx};
        const inputs = wrap.querySelectorAll(input[name="${name}"]);
        const chosen = [...inputs].find(i => i.checked);
        wrap.classList.remove("correct","wrong");
        if (chosen) {
          answered++;
          const isRight = Number(chosen.value) === list[idx].correct;
          if (isRight) { correct++; wrap.classList.add("correct"); }
          else { wrap.classList.add("wrong"); }
        }
      });

      result.hidden = false;
      result.textContent = Ответы: ${answered} из ${list.length}. Правильных: ${correct}. Ошибок: ${answered - correct}.;
      result.scrollIntoView({ behavior: "smooth", block: "center" });
    }

    function resetQuiz(mountId, resultId) {
      const mount = document.getElementById(mountId);
      const result = document.getElementById(resultId);
      [...mount.querySelectorAll('input[type="radio"]')].forEach(i => i.checked = false);
      [...mount.children].forEach(w => w.classList.remove("correct","wrong"));
      result.hidden = true;
      result.textContent = "";
    }

    // Инициализация
    renderQuiz(ww2, "ww2-questions", "ww2");
    renderQuiz(afg, "afg-questions", "afg");

    document.getElementById("check-ww2").addEventListener("click", () => checkQuiz(ww2, "ww2-questions", "result-ww2", "ww2"));
    document.getElementById("reset-ww2").addEventListener("click", () => resetQuiz("ww2-questions", "result-ww2"));

    document.getElementById("check-afg").addEventListener("click", () => checkQuiz(afg, "afg-questions", "result-afg", "afg"));
    document.getElementById("reset-afg").addEventListener("click", () => resetQuiz("afg-questions", "result-afg"));
  </script>
</body>
</html>
