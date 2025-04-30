---
layout: page 
title: Science Questions
search_exclude: true
permalink: /sciencequestions/
---

<div class="min-h-screen bg-gray-700 text-white flex items-center justify-center px-4 py-16">
  <div class="bg-gray-800 rounded-2xl shadow-2xl p-8 max-w-lg w-full text-center border border-gray-600">
    <h2 class="text-2xl font-bold text-green-300 border-b-4 border-green-300 pb-2 mb-6">
      Science Question Predictor
    </h2>
    <input 
      type="text" 
      id="science-question-input" 
      placeholder="Enter a science question..." 
      class="w-full px-4 py-2 mb-4 rounded-md text-white bg-gray-900 placeholder-gray-400 border border-gray-600 focus:outline-none focus:ring-2 focus:ring-green-400"
    />
    <button 
      id="send-question-btn" 
      class="w-full bg-green-500 text-white font-bold py-2 px-4 rounded-xl hover:bg-green-300 hover:text-gray-900 transition transform hover:scale-105 active:scale-95"
    >
      Predict Topic
    </button>
    <div id="result" class="mt-6 text-left hidden bg-gray-900 bg-opacity-80 rounded-lg p-4 border border-green-500"></div>
  </div>
</div>

<script type="module">
import { pythonURI, fetchOptions } from '../assets/js/api/config.js';

async function sendScienceQuestion() {
    const question = document.getElementById('science-question-input').value.trim();
    const resultDiv = document.getElementById('result');

    if (question === '') {
        alert("Please enter a question.");
        return;
    }

    resultDiv.classList.remove("hidden");
    resultDiv.innerHTML = "Predicting...";

    try {
        const response = await fetch(pythonURI + '/api/predict', {
            ...fetchOptions,
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ question: question })
        });

        if (!response.ok) {
            throw new Error('Prediction failed: ' + response.statusText);
        }

        const data = await response.json();
        const topic = data.predicted_topic.charAt(0).toUpperCase() + data.predicted_topic.slice(1);
        const probabilities = data.probabilities;

        let formattedProbs = '';
        for (let key in probabilities) {
            if (probabilities.hasOwnProperty(key)) {
                const percent = (probabilities[key] * 100).toFixed(2);
                const label = key.charAt(0).toUpperCase() + key.slice(1);
                formattedProbs += `<li class="mb-1"><strong>${label}:</strong> ${percent}%</li>`;
            }
        }

        resultDiv.innerHTML = `
            <p><strong>Predicted Topic:</strong> ${topic}</p>
            <p class="mt-2"><strong>Confidence Levels:</strong></p>
            <ul class="list-none pl-0">${formattedProbs}</ul>
        `;
    } catch (error) {
        console.error('Error:', error);
        resultDiv.innerHTML = '<strong>Error:</strong> ' + error.message;
    }
}

document.getElementById('send-question-btn').addEventListener('click', sendScienceQuestion);
</script>
