<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Lab Adventure</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f9fafb;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      margin: 0;
      padding: 20px;
    }
    .container {
      background: white;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      max-width: 500px;
      width: 100%;
      text-align: center;
    }
    h2 {
      margin-bottom: 20px;
      font-size: 24px;
    }
    button {
      margin: 10px;
      padding: 10px 20px;
      border: none;
      border-radius: 8px;
      background-color: #4f46e5;
      color: white;
      font-size: 16px;
      cursor: pointer;
      transition: background 0.3s;
    }
    button:hover {
      background-color: #4338ca;
    }
    input {
      padding: 10px;
      width: 80%;
      margin-top: 10px;
      margin-bottom: 20px;
      border: 1px solid #ccc;
      border-radius: 8px;
    }
  </style>
</head>
<body>

<div class="container" id="lab">
  <h2 id="scenario">Loading...</h2>
  <div id="options"></div>
  <div id="gameOver" style="display:none;">
    <h2>Lab Over!</h2>
    <p id="score"></p>
    <input type="text" id="nameInput" placeholder="Enter your name" />
    <br>
    <button onclick="submitScore()">Submit Score</button>
    <p id="submitMessage"></p>
  </div>
</div>

<script>
  const questions = [
    {
      scenario: 'You are extracting DNA. Do you add ethanol or water to precipitate the DNA?',
      options: ['Ethanol', 'Water'],
      correct: 'Ethanol',
    },
    {
      scenario: 'You need to separate proteins by size. Do you use gel electrophoresis or spectrophotometry?',
      options: ['Gel Electrophoresis', 'Spectrophotometry'],
      correct: 'Gel Electrophoresis',
    },
    {
      scenario: 'You want to amplify a DNA segment. Do you use PCR or CRISPR?',
      options: ['PCR', 'CRISPR'],
      correct: 'PCR',
    },
    // Add more questions here
  ];

  let currentQuestion = 0;
  let points = 0;

  function loadQuestion() {
    if (currentQuestion < questions.length) {
      document.getElementById('scenario').innerText = questions[currentQuestion].scenario;
      const optionsDiv = document.getElementById('options');
      optionsDiv.innerHTML = '';

      questions[currentQuestion].options.forEach(option => {
        const btn = document.createElement('button');
        btn.innerText = option;
        btn.onclick = () => checkAnswer(option);
        optionsDiv.appendChild(btn);
      });
    } else {
      endGame();
    }
  }

  function checkAnswer(selected) {
    if (selected === questions[currentQuestion].correct) {
      points++;
      currentQuestion++;
      loadQuestion();
    } else {
      endGame();
    }
  }

  function endGame() {
    document.getElementById('scenario').style.display = 'none';
    document.getElementById('options').style.display = 'none';
    document.getElementById('gameOver').style.display = 'block';
    document.getElementById('score').innerText = `You scored ${points} points!`;
  }

  function submitScore() {
    const name = document.getElementById('nameInput').value;
    if (name.trim() === '') {
      alert('Please enter your name.');
      return;
    }

    fetch(pythonURI + "/api/binary-history", { // Send a POST request to add the event
        method: "POST", // Use POST method
        headers: {
            "Content-Type": "application/json", // Set request headers
        },
      body: JSON.stringify({ name: name, points: points })
    })
    .then(response => {
      if (response.ok) {
        document.getElementById('submitMessage').innerText = 'Score submitted successfully!';
      } else {
        document.getElementById('submitMessage').innerText = 'Failed to submit score.';
      }
    })
    .catch(error => {
      console.error('Error submitting score:', error);
      document.getElementById('submitMessage').innerText = 'Error submitting score.';
    });
  }

  loadQuestion();
</script>

</body>
</html>