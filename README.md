<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quel personnage de Batman es-tu ?</title>
  <style>
    body {
      font-family: 'Courier New', monospace;
      background-color: #0d0d0d;
      color: #f0f0f0;
      margin: 0;
      padding: 20px;
      text-align: center;
      overflow-x: hidden;
    }

    h1 {
      color: #ffcc00;
      text-shadow: 2px 2px 5px #000;
      font-size: 40px;
      margin-bottom: 20px;
    }

    img.logo {
      max-width: 200px;
      margin-bottom: 20px;
    }

    .question {
      margin: 20px 0;
      background-color: #1a1a1a;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(255, 204, 0, 0.5);
    }

    .options button {
      background-color: #ffcc00;
      color: #000;
      padding: 10px 20px;
      border: none;
      margin: 5px;
      font-size: 18px;
      cursor: pointer;
      border-radius: 5px;
      transition: all 0.3s ease;
      position: relative;
      overflow: hidden;
    }

    .options button:hover {
      background-color: #ffa500;
      transform: scale(1.1);
    }

    .bat {
      position: absolute;
      width: 20px;
      height: 20px;
      background: url('https://upload.wikimedia.org/wikipedia/commons/thumb/5/55/Bat_silhouette.svg/32px-Bat_silhouette.svg.png') no-repeat center/contain;
      animation: fly 1s linear forwards;
      pointer-events: none;
    }

    @keyframes fly {
      0% { transform: translateY(0) scale(1); opacity: 1; }
      100% { transform: translateY(-200px) scale(1.5); opacity: 0; }
    }

    .result {
      background-color: #1a1a1a;
      padding: 20px;
      border-radius: 10px;
      margin-top: 20px;
      display: none;
      box-shadow: 0 0 10px rgba(255, 204, 0, 0.5);
    }

    .score {
      font-size: 18px;
      margin-top: 10px;
      color: #ffcc00;
    }
  </style>
</head>
<body>
  <img class="logo" src="https://zupimages.net/up/25/16/71ta.png" alt="Logo Batman">
  <h1>Quel personnage de Batman es-tu ?</h1>

  <div id="quiz-container">
    <div class="question" id="question-container">
      <h2 id="question-text"></h2>
      <div class="options" id="options-container"></div>
    </div>
  </div>

  <div class="result" id="result-container">
    <h2>Tu es...</h2>
    <p id="result-text"></p>
    <p class="score">Résultat 100% Gotham approved.</p>
    <button onclick="startQuiz()">Refaire le quiz</button>
  </div>

  <script>
    const questions = [...]; // questions comme avant (non modifiées ici pour lisibilité)
    const characters = [...]; // personnages comme avant (non modifiés ici pour lisibilité)

    let currentQuestion = 0;
    let userAnswers = [];

    function startQuiz() {
      currentQuestion = 0;
      userAnswers = [];
      document.getElementById('result-container').style.display = 'none';
      document.getElementById('quiz-container').style.display = 'block';
      showQuestion();
    }

    function showQuestion() {
      const question = questions[currentQuestion];
      document.getElementById('question-text').innerText = question.question;

      const optionsContainer = document.getElementById('options-container');
      optionsContainer.innerHTML = '';

      question.options.forEach((option, index) => {
        const button = document.createElement('button');
        button.innerText = option;
        button.onclick = () => {
          userAnswers.push(question.result[index]);
          spawnBats(button);
          setTimeout(() => {
            currentQuestion++;
            if (currentQuestion < questions.length) {
              showQuestion();
            } else {
              showResult();
            }
          }, 500);
        };
        optionsContainer.appendChild(button);
      });
    }

    function showResult() {
      const resultIndex = userAnswers.reduce((a, b) => a + b, 0) % characters.length;
      const result = characters[resultIndex];
      document.getElementById('result-text').innerText = `${result.name} - ${result.description}`;
      document.getElementById('quiz-container').style.display = 'none';
      document.getElementById('result-container').style.display = 'block';
    }

    function spawnBats(button) {
      for (let i = 0; i < 10; i++) {
        const bat = document.createElement('div');
        bat.classList.add('bat');
        bat.style.left = `${Math.random() * 100}%`;
        bat.style.top = `${Math.random() * 100}%`;
        button.appendChild(bat);
        setTimeout(() => bat.remove(), 1000);
      }
    }

    startQuiz();
  </script>
</body>
</html>
