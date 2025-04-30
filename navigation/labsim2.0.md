---
layout: base 
title: Lab Simulation 2.0
search_exclude: true
permalink: /labsim2.0/
---

<div class="min-h-screen bg-gradient-to-br from-[#0E3348] via-[#247994] to-[#0F547B] text-white flex justify-center items-center p-6">
  <div class="bg-gray-900 bg-opacity-70 p-8 rounded-xl shadow-xl w-full max-w-lg text-center" id="lab">
    <h2 id="scenario" class="text-2xl font-bold text-white border-b-4 border-white pb-2 mb-4">Loading...</h2>
    <div id="options" class="flex flex-col space-y-4"></div>

    <div id="gameOver" class="hidden">
      <h2 class="text-2xl font-bold text-white border-b-4 border-white pb-2 mb-2">Lab Over!</h2>
      <p id="score" class="mb-4"></p>
      <input type="text" id="nameInput" placeholder="Enter your name" class="p-2 rounded-md w-4/5 text-black mb-4" />
      <button onclick="submitScore()" class="bg-gray-800 text-white font-bold py-2 px-4 rounded-xl hover:bg-gray-700 active:bg-gray-900 w-4/5 mx-auto">Submit Score</button>
      <p id="submitMessage" class="mt-2"></p>
      <button id="playAgainButton" onclick="restartLab()" class="bg-gray-800 text-white font-bold py-2 px-4 rounded-xl hover:bg-gray-700 active:bg-gray-900 mt-4 hidden w-4/5 mx-auto">Play Again</button>
    </div>
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
    const scenario = document.getElementById('scenario');
    const optionsDiv = document.getElementById('options');
    const gameOver = document.getElementById('gameOver');

    scenario.style.display = 'block';
    optionsDiv.style.display = 'block';
    gameOver.classList.add('hidden');
    optionsDiv.innerHTML = '';

    if (currentQuestion >= questions.length) {
      endLab();
      return;
    }

    scenario.textContent = questions[currentQuestion].scenario;

    questions[currentQuestion].options.forEach(option => {
      const button = document.createElement('button');
      button.textContent = option;
      button.className = "bg-gray-800 text-white font-bold py-2 px-4 rounded-xl hover:bg-gray-700 active:bg-gray-900 w-4/5 mx-auto";
      button.onclick = () => checkAnswer(option);
      optionsDiv.appendChild(button);
    });
  }

  function checkAnswer(selected) {
    if (selected === questions[currentQuestion].correct) {
      points++;
      currentQuestion++;
      if (currentQuestion === questions.length) {
        document.getElementById('lab').innerHTML = `
          <h2 class="text-2xl font-bold text-white border-b-4 border-white pb-2 mb-2">You Win!</h2>
          <p id="score" class="mb-4">Points: ${points}</p>
          <input type="text" id="nameInput" placeholder="Enter your name" class="p-2 rounded-md w-4/5 text-black mb-4" />
          <button onclick="submitScore()" class="bg-gray-800 text-white font-bold py-2 px-4 rounded-xl hover:bg-gray-700 active:bg-gray-900 w-4/5 mx-auto">Submit Score</button>
          <p id="submitMessage" class="mt-2"></p>
        `;
      } else {
        loadQuestion();
      }
    } else {
      document.getElementById('scenario').style.display = 'none';
      document.getElementById('options').style.display = 'none';
      const gameOver = document.getElementById('gameOver');
      gameOver.classList.remove('hidden');
      document.getElementById('score').textContent = `Points: ${points}`;
    }
  }

  import { pythonURI, fetchOptions } from '../assets/js/api/config.js';

  async function submitScore() {
    const name = document.getElementById('nameInput').value;
    if (name.trim() === '') {
      alert('Please enter your name.');
      return;
    }

    const resultsData = {
      name: name,
      points: points,
    };

    try {
      const response = await fetch(pythonURI + "/api/labsim", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(resultsData)
      });

      if (response.ok) {
        alert(`Thank you, ${name}! Your score was submitted.`);
        document.getElementById('playAgainButton').classList.remove('hidden');
        document.getElementById('nameInput').value = '';
        currentQuestion = 0;
        points = 0;
        loadQuestion();
      } else {
        throw new Error('Network response failed');
      }
    } catch (error) {
      console.error("There was a problem with the fetch", error);
    }
  }

  function restartLab() {
    currentQuestion = 0;
    points = 0;
    document.getElementById('gameOver').classList.add('hidden');
    document.getElementById('nameInput').value = '';
    document.getElementById('submitMessage').textContent = '';
    document.getElementById('playAgainButton').classList.add('hidden');
    document.getElementById('scenario').style.display = 'block';
    document.getElementById('options').style.display = 'block';
    loadQuestion();
  }

  window.submitScore = submitScore;
  window.restartLab = restartLab;
  loadQuestion();
</script>
