---
layout: base 
title: Lab Simulation 2.0
search_exclude: true
permalink: /labsim2.0/
---


<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Science Trivia</title>
  <style>
    body {
        background: linear-gradient(150deg,rgb(89, 82, 0),rgb(255, 234, 0),rgb(255, 238, 160));
        color:rgb(4, 2, 2);
        font-family: Arial, sans-serif;
        min-height: 100vh;
        margin: 20px;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow-y: auto;
    }
    .container {
        background: rgba(255, 255, 255, 0.1);
        padding: 30px;
        border-radius: 12px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        max-width: 500px;
        width: 100%;
        text-align: center;
    }
    h2, h3 {
        color: rgb(0, 0, 0);
        border-bottom: 4px solid #000000;
        padding: 10px;
        margin-bottom: 20px;
    }
    #options {
        display: flex;
        flex-direction: column;
        gap: 20px;
        margin-top: 20px;
    }
    .button {
        all: unset;
        background-color: white !important;
        border: 2px solid #ccc;
        border-radius: 12px;
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
        transition: background-color 0.3s ease, transform 0.1s ease;
        font-weight: bold;
        color: black !important;
        margin: 0 auto;
        width: 80%;
        text-align: center;
    }
    .button:hover {
        background-color: gray !important;
        transform: scale(1.05);
    }
    .button:active {
        background-color: darkgrey !important;
        transform: scale(0.95);
    }
    input {
        padding: 10px;
        width: 80%;
        margin-top: 10px;
        margin-bottom: 20px;
        border: 1px solid #ccc;
        border-radius: 8px;
    }
    #difficulty {
        color: black;
        font-size: 18px;
        font-weight: bold;
        margin-bottom: 10px;
    }
  </style>
</head>
<body>

<div class="container" id="lab">
  <h2 id="scenario">Loading...</h2>
  <p id="difficulty"></p>
  <div id="options"></div>
  <div id="gameOver" style="display:none;">
    <h2>Game Over!</h2>
    <p id="score"></p>
    <input type="text" id="nameInput" placeholder="Enter your name" />
    <br>
    <button onclick="submitScore()">Submit Score</button>
    <p id="submitMessage"></p>
    <button id="playAgainButton" style="display:none; margin-top: 10px;" onclick="restartLab()">Play Again</button>
  </div>
</div>

<script>
// Define the backend URI (similar to your previous implementation)
var pythonURI;
if (location.hostname === "localhost") {
        pythonURI = "http://localhost:8887";
} else if (location.hostname === "127.0.0.1") {
        pythonURI = "http://127.0.0.1:8887";
} else {
        pythonURI =  "https://flocker.nighthawkcodingsociety.com";
}

let questions = [];
let currentDifficulty = 3;
let points = 0;
let correctStreak = 0;
let wrongStreak = 0;

// Fetch questions
async function fetchQuestions() {
  try {
    console.log("Fetching questions...");
    const response = await fetch(`${pythonURI}/api/questions`);
    if (!response.ok) {
      throw new Error(`Failed to fetch questions: ${response.status}`);
    }
    const data = await response.json();
    if (!Array.isArray(data) || data.length === 0) {
      throw new Error("No questions found in the response.");
    }
    questions = data.map(q => ({
      question: q.scenario,
      distractor1: q.options[1],
      distractor2: q.options[2],
      distractor3: q.options[0],
      correct_answer: q.answer,
      difficulty_rating: q.difficulty
    }));
    console.log("Questions fetched:", questions);
    loadQuestion();
  } catch (error) {
    console.error('Error fetching questions:', error);
    document.getElementById('scenario').textContent = "Hmm, the server seems to be down. Please try again later.";
  }
}

function getQuestionByDifficulty(difficulty) {
  const filtered = questions.filter(q => parseInt(q.difficulty_rating) === difficulty);
  return filtered[Math.floor(Math.random() * filtered.length)];
}

function loadQuestion() {
  console.log("Loading question...");
  const currentQuestion = getQuestionByDifficulty(currentDifficulty);
  console.log("Current question:", currentQuestion);

  if (!currentQuestion) {
    endLab();
    return;
  }

  document.getElementById('scenario').textContent = currentQuestion.question;
  document.getElementById('difficulty').textContent = `Difficulty Level: ${currentQuestion.difficulty_rating}`;
  const optionsDiv = document.getElementById('options');
  optionsDiv.innerHTML = '';

  const options = [
    currentQuestion.correct_answer,
    currentQuestion.distractor1,
    currentQuestion.distractor2,
    currentQuestion.distractor3
  ].sort(() => Math.random() - 0.5);

  options.forEach(option => {
    const button = document.createElement('button');
    button.textContent = option;
    button.classList.add('button');
    button.onclick = () => checkAnswer(option, currentQuestion.correct_answer);
    optionsDiv.appendChild(button);
  });
}

function endLab() {
  document.getElementById('scenario').textContent = "No more questions available.";
  document.getElementById('options').innerHTML = '';
}

function checkAnswer(selected, correct) {
  if (selected === correct) {
    points++;
    correctStreak++;
    wrongStreak = 0; // Reset wrong streak on a correct answer
    if (currentDifficulty < 5) currentDifficulty++;
    if (correctStreak === 3 && currentDifficulty === 5) {
      winGame();
      return;
    }
  } else {
    correctStreak = 0;
    if (currentDifficulty > 1) {
      currentDifficulty--; // Decrease difficulty
      wrongStreak = 0; // Reset wrong streak when difficulty changes
    } else {
      wrongStreak++; // Increment wrong streak only at difficulty level 1
      if (wrongStreak === 3) {
        loseGame();
        return;
      }
    }
  }
  loadQuestion();
}

function winGame() {
  document.getElementById('scenario').style.display = 'none';
  document.getElementById('options').style.display = 'none';
  document.getElementById('gameOver').style.display = 'block';
  document.getElementById('score').textContent = `Points: ${points}`;
  document.getElementById('gameOver').querySelector('h2').textContent = 'You Win!';
}

function loseGame() {
  document.getElementById('scenario').style.display = 'none';
  document.getElementById('options').style.display = 'none';
  document.getElementById('gameOver').style.display = 'block';
  document.getElementById('score').textContent = `Points: ${points}`;
  document.getElementById('gameOver').querySelector('h2').textContent = 'Game Over!';
}

function restartLab() {
  currentDifficulty = 3;
  points = 0;
  correctStreak = 0;
  wrongStreak = 0;
  document.getElementById('gameOver').style.display = 'none';
  document.getElementById('nameInput').value = '';
  document.getElementById('submitMessage').textContent = '';
  document.getElementById('playAgainButton').style.display = 'none';
  document.getElementById('scenario').style.display = 'block';
  document.getElementById('options').style.display = 'block';
  loadQuestion();
}

// Submit score
async function submitScore() {
      const name = document.getElementById('nameInput').value;
      if (name.trim() === '') {
          alert('Please enter your name.');
          return;
      }
      const resultsData = { // Create an object with the event data
          name: name,
          points: points,
      };
      try {
          const response = await fetch(pythonURI + "/api/labsim", { // Send a POST request
              method: "POST", // Use POST method
              headers: {
                  "Content-Type": "application/json", // Set request headers
              },
              body: JSON.stringify(resultsData)
          });

          if (response.ok) {
              alert(`Thank you, ${name}! Your score was submitted.`);
              document.getElementById('playAgainButton').style.display = 'inline-block';
          } else {
              throw new Error('Network response failed');
          }
      } catch (error) { // Handle any errors during fetch
          console.error("There was a problem with the fetch", error);
      }
}

fetchQuestions(); // Start by fetching questions
window.submitScore = submitScore;
window.restartLab = restartLab;

</script>

</body>
</html>