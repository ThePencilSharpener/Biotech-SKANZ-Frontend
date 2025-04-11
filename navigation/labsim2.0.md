---
layout: base 
title: Lab Simulation 2.0
search_exclude: true
permalink: /labsim2.0/
---


<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Lab Adventure</title>
  <style>
    body {
        background: linear-gradient(150deg, #0E3348, #247994, #147EA0, #0F547B);
        color: #ffffff;
        font-family: Arial, sans-serif;
        min-height: 100vh;
        margin: 20px;
        display: flex;
        justify-content: center;
        align-items: center;
        overflow-y: auto;
    }
    .container {
        background: ;
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
        padding: 10px; /* Space around the text */
        margin-bottom: 20px
    }
    #options {
        display: flex;
        flex-direction: column;
        gap: 20px; /* Adds space between the two buttons */
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
        margin: 0 auto; /* Center the button */
        width: 80%; /* Optional: Make all buttons same width */
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
    <button id="playAgainButton" style="display:none; margin-top: 10px;" onclick="restartLab()">Play Again</button>
  </div>
</div>

<script type="module" defer>
  const questions = [
    {
        scenario: 'You are isolating DNA. To make the DNA strands visible, do you add ethanol or water?',
        options: ['Ethanol', 'Water'],
        correct: 'Ethanol',
    },
    {
        scenario: 'You need to copy a specific segment of DNA. Do you use PCR or gel electrophoresis?',
        options: ['PCR', 'Gel Electrophoresis'],
        correct: 'PCR',
    },
    {
        scenario: 'You are designing an mRNA vaccine. Do you code the mRNA for an antigen or an antibody?',
        options: ['Antibody', 'Antigen'],
        correct: 'Antigen',
    },
    {
        scenario: 'When testing a vaccine’s safety, do you first conduct animal trials or human trials?',
        options: ['Animal Trials', 'Human Trials'],
        correct: 'Animal Trials',
    },
    {
        scenario: 'You want to check the quality of extracted DNA. Should you use a spectrophotometer or a microscope?',
        options: ['Microscope', 'Spectrophotometer'],
        correct: 'Spectrophotometer',
    },
    {
        scenario: 'You are making a DNA vaccine. Should you inject the patient with plasmid DNA or viral RNA?',
        options: ['Viral RNA', 'Plasmid DNA'],
        correct: 'Plasmid DNA',
    },
    {
        scenario: 'After receiving a vaccine, your body produces memory cells. Are these B cells or red blood cells?',
        options: ['Red blood cells', 'B cells'],
        correct: 'B cells',
    },
  ];

  let currentQuestion = 0;
  let points = 0;

  function loadQuestion() {
      const lab = document.getElementById('lab');
      const scenario1 = document.getElementById('scenario');
      const options1 = document.getElementById('options');

      // Clear previous options
      options1.innerHTML = '';

      // Hide game over screen if showing
      document.getElementById('gameOver').style.display = 'none';
      scenario1.style.display = 'block';
      options1.style.display = 'block';

      // Check if out of questions
      if (currentQuestion >= questions.length) {
        endLab();
        return;
      }

      // Set scenario
      scenario1.textContent = questions[currentQuestion].scenario;

      // Create new option buttons
      questions[currentQuestion].options.forEach(option => {
        const button = document.createElement('button');
        button.textContent = option;
        button.onclick = () => checkAnswer(option);
  
        // Style the button
        button.style.display = 'block';    // make it stack vertically
        button.style.margin = '10px auto'; // 10px margin top/bottom, auto left/right to center
        button.style.width = '80%';       // make button 80px wide
        button.style.textAlign = 'center'; // center the text inside the button

        options1.appendChild(button);
      });
  }

  function checkAnswer(selected) {
    if (selected === questions[currentQuestion].correct) {
      points++;
      currentQuestion++;
      if (currentQuestion === questions.length) {
        // If it's the last question and it's correct
        document.getElementById('lab').innerHTML = `
          <h2>You Win!</h2>
          <p id="score">Points: ${points}</p>
          <input type="text" id="nameInput" placeholder="Enter your name" />
          <br>
          <button onclick="submitScore()">Submit Score</button>
          <p id="submitMessage"></p>
        `;
      } else {
        loadQuestion();
      }
    } else {
      // Wrong answer
      document.getElementById('scenario').style.display = 'none';
      document.getElementById('options').style.display = 'none';
      document.getElementById('gameOver').style.display = 'block';
      document.getElementById('score').textContent = `Points: ${points}`;
    }
  }

  import { pythonURI, fetchOptions } from '../assets/js/api/config.js';

  function restartLab() {
      console.log("restartLab called");
      currentQuestion = 0;
      points = 0;
      document.getElementById('gameOver').style.display = 'none';
      document.getElementById('nameInput').value = '';
      document.getElementById('submitMessage').textContent = '';
      document.getElementById('playAgainButton').style.display = 'none';
      document.getElementById('scenario').style.display = 'block';
      document.getElementById('options').style.display = 'block';
      loadQuestion();  // 🚀 restart with the first question
  }

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
      
              // Clear the name input field
              document.getElementById('nameInput').value = '';

              // Reset currentQuestion and points to start fresh for next play
              currentQuestion = 0;
              points = 0;

              // Reload the first question
              loadQuestion();
          } else {
              throw new Error('Network response failed');
          }
      } catch (error) { // Handle any errors during fetch
          console.error("There was a problem with the fetch", error);
      }
  }
  window.submitScore = submitScore;
  window.restartLab = restartLab;
  loadQuestion();

</script>

</body>
</html>