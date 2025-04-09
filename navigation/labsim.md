---
layout: base 
title: Lab Simulation
search_exclude: true
permalink: /labsim/
---

<img src="https://private-user-images.githubusercontent.com/179050906/430102645-15a8e99c-4277-4702-82d1-7101ff657d7a.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDQxNjQzNTksIm5iZiI6MTc0NDE2NDA1OSwicGF0aCI6Ii8xNzkwNTA5MDYvNDMwMTAyNjQ1LTE1YThlOTljLTQyNzctNDcwMi04MmQxLTcxMDFmZjY1N2Q3YS5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUwNDA5JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MDQwOVQwMjAwNTlaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1lNDZlZGFjMzlkZTAzODZmYmFjMzE0N2ZhNGI3ZTU5ZWZiNTAwNjBmYmZlZWQxN2VlNWNiOTgxZjBhZGY0NzE5JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.b2VAeIRJyf4ihbeg4Y2QJrmPSuAnX4EkMfiYJ_6pk2g" alt="Gafame">




<!-- Your container (optional) -->
<div id="game-area" style="position: relative; width: 800px; height: 600px; border: 2px solid black;">
  </button>
  <button id="myButton" style="position: absolute; left: 570px; top: -590px;">Click Me</button>
</div>

<script>
  document.getElementById('myButton').addEventListener('click', () => {
    alert("🎯 You clicked the button at (250, 400)!");
  });
</script>
<script type="module">
import { pythonURI, fetchOptions } from '{{ site.baseurl }}/assets/js/api/config.js';

async function fetchLabSims(customURL = `${pythonURI}/api/labsim`) {
    try {
        const response = await fetch(customURL, {
            ...fetchOptions,
            method: 'GET'
        });
        if (!response.ok) {
            throw new Error('Failed to fetch lab sims: ' + response.statusText);
        }
        const data = await response.json();
        const labSimList = document.getElementById('labsim-list');
        labSimList.innerHTML = "";
        data.forEach(sim => {
            const listItem = document.createElement('li');
            listItem.style.display = 'flex';
            listItem.style.alignItems = 'center';
            listItem.style.justifyContent = 'space-between';

            const simText = document.createElement('span');
            simText.textContent = `${sim.age}: ${sim.dna}`;

            const updateButton = document.createElement('button');
            updateButton.textContent = 'Update';
            updateButton.style.marginLeft = '10px';
            updateButton.style.padding = '2px 5px';
            updateButton.onclick = () => promptUpdateLabSim(sim.id, sim.age, sim.dna);

            const deleteButton = document.createElement('button');
            deleteButton.textContent = 'Delete';
            deleteButton.style.marginLeft = '10px';
            deleteButton.style.padding = '2px 5px';
            deleteButton.onclick = () => deleteLabSim(sim.id);

            listItem.appendChild(simText);
            listItem.appendChild(updateButton);
            listItem.appendChild(deleteButton);
            labSimList.appendChild(listItem);
        });
    } catch (error) {
        console.error('Error fetching lab sims:', error);
    }
}

async function addLabSim() {
    const age = document.getElementById('new-sim-age').value;
    const dna = document.getElementById('new-sim-dna').value;
    try {
        const response = await fetch(`${pythonURI}/api/labsim`, {
            ...fetchOptions,
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ age, dna })
        });
        if (!response.ok) {
            throw new Error('Failed to add lab sim: ' + response.statusText);
        }
        alert('Lab sim added successfully!');
        document.getElementById('new-sim-age').value = '';
        document.getElementById('new-sim-dna').value = '';
        fetchLabSims();
    } catch (error) {
        console.error('Error adding lab sim:', error);
        alert('Error adding lab sim: ' + error.message);
    }
}

async function promptUpdateLabSim(id, age, dna) {
    const newAge = prompt('Enter new age:', age);
    const newDna = prompt('Enter new DNA:', dna);
    if (newAge && newDna) {
        try {
            const response = await fetch(`${pythonURI}/api/labsim`, {
                ...fetchOptions,
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ age, dna, new_age: newAge, new_dna: newDna })
            });
            if (!response.ok) {
                throw new Error('Failed to update lab sim: ' + response.statusText);
            }
            alert('Lab sim updated successfully!');
            fetchLabSims();
        } catch (error) {
            console.error('Error updating lab sim:', error);
            alert('Error updating lab sim: ' + error.message);
        }
    }
}

async function deleteLabSim(id) {
    try {
        const response = await fetch(`${pythonURI}/api/labsim`, {
            ...fetchOptions,
            method: 'DELETE',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ id })
        });
        if (!response.ok) {
            throw new Error('Failed to delete lab sim: ' + response.statusText);
        }
        alert('Lab sim deleted successfully!');
        fetchLabSims();
    } catch (error) {
        console.error('Error deleting lab sim:', error);
        alert('Error deleting lab sim: ' + error.message);
    }
}
</script>
