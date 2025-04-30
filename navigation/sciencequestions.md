---
layout: base 
title: Science Questions
search_exclude: true
permalink: /sciencequestions/
---

<title>Science Question Predictor</title>
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
        background: rgba(255, 255, 255, 0.1);
        padding: 30px;
        border-radius: 12px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.3);
        max-width: 500px;
        width: 100%;
        text-align: center;
    }
    h2 {
        color: #ffffff;
        border-bottom: 4px solid #ffffff;
        padding: 10px;
        margin-bottom: 20px;
    }
    input[type="text"] {
        padding: 10px;
        width: 80%;
        margin-top: 10px;
        margin-bottom: 20px;
        border: 1px solid #ccc;
        border-radius: 8px;
        font-size: 16px;
    }
    .button {
        all: unset;
        background-color: white;
        border: 2px solid #ccc;
        border-radius: 12px;
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
        transition: background-color 0.3s ease, transform 0.1s ease;
        font-weight: bold;
        color: black;
        margin: 0 auto;
        width: 80%;
        text-align: center;
    }
    .button:hover {
        background-color: gray;
        transform: scale(1.05);
    }
    .button:active {
        background-color: darkgray;
        transform: scale(0.95);
    }
    #result {
        margin-top: 20px;
        font-size: 18px;
        color: #fff;
        background-color: rgba(0,0,0,0.3);
        padding: 10px;
        border-radius: 10px;
        display: none;
        text-align: left;
    }
</style>
<body>
<div class="container">
<h2>Science Question Predictor</h2>
<input type="text" id="science-question-input" placeholder="Enter a science question..." />
<br>
<button class="button" id="send-question-btn">Predict Topic</button>
<div id="result"></div>
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

    resultDiv.style.display = 'block';
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

        var formattedProbs = '';
        for (var key in probabilities) {
            if (probabilities.hasOwnProperty(key)) {
                var percent = (probabilities[key] * 100).toFixed(2);
                var label = key.charAt(0).toUpperCase() + key.slice(1);
                formattedProbs += '<li><strong>' + label + ':</strong> ' + percent + '%</li>';
            }
        }

        resultDiv.innerHTML = 
            '<p><strong>Predicted Topic:</strong> ' + topic + '</p>' +
            '<p><strong>Confidence Levels:</strong></p>' +
            '<ul style="list-style: none; padding: 0;">' + formattedProbs + '</ul>';
    } catch (error) {
        console.error('Error:', error);
        resultDiv.innerHTML = '<strong>Error:</strong> ' + error.message;
    }
}

document.getElementById('send-question-btn').addEventListener('click', sendScienceQuestion);
</script>
</body>
