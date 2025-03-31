<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Student Voting</title>
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background-color: #f8f9fa;
            color: #343a40;
        }
        h1 {
            margin-top: 20px;
            font-size: 2.5em;
            color: black; /* Changed color to black */
        }
        .options {
            display: flex;
            justify-content: center;
            margin-top: 20px;
        }
        .options button {
            margin: 10px;
            padding: 15px 30px;
            font-size: 18px;
            background-color: #007bff;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        .options button:hover {
            background-color: #0056b3;
        }
        .results {
            margin-top: 30px;
        }
        button.reset {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
            background-color: #dc3545;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        button.reset:hover {
            background-color: #c82333;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            background-color: white;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap" rel="stylesheet">
</head>
<body>
    <div class="container">
        <h1 id="title">Student Voting</h1>
        <div class="options">
            <button onclick="vote('fact')" aria-label="Vote for Fact">Fact</button>
            <button onclick="vote('opinion')" aria-label="Vote for Opinion">Opinion</button>
            <button onclick="vote('unknown')" aria-label="Vote for I don't know">I don't know</button>
        </div>
        <div class="results">
            <canvas id="resultsChart"></canvas>
        </div>
        <button class="reset" onclick="confirmReset()">Reset</button>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        let votes = { fact: 0, opinion: 0, unknown: 0 };
        let chart;
        const hasVotedKey = 'hasVoted';

        function vote(option) {
            if (!localStorage.getItem(hasVotedKey)) {
                votes[option]++;
                localStorage.setItem(hasVotedKey, 'true');
                updateChart();
            } else {
                alert('You have already voted. Please wait for the next reset to vote again.');
            }
        }

        function confirmReset() {
            const password = prompt("Enter the password to reset votes:");
            if (password === "eng") {
                const newTitle = prompt("Enter the new title:");
                if (newTitle) {
                    document.getElementById('title').textContent = newTitle;
                }
                resetVotes();
            } else {
                alert("Incorrect password.");
            }
        }

        function resetVotes() {
            votes = { fact: 0, opinion: 0, unknown: 0 };
            localStorage.removeItem(hasVotedKey);
            updateChart();
        }

        function updateChart() {
            const ctx = document.getElementById('resultsChart').getContext('2d');
            if (chart) {
                chart.data.datasets[0].data = [votes.fact, votes.opinion, votes.unknown];
                chart.update();
            } else {
                chart = new Chart(ctx, {
                    type: 'bar',
                    data: {
                        labels: ['Fact', 'Opinion', 'I don\'t know'],
                        datasets: [{
                            label: 'Votes',
                            data: [votes.fact, votes.opinion, votes.unknown],
                            backgroundColor: ['#007bff', '#28a745', '#ffc107']
                        }]
                    },
                    options: {
                        scales: {
                            y: {
                                beginAtZero: true,
                                max: 15,
                                ticks: {
                                    stepSize: 1
                                }
                            }
                        }
                    }
                });
            }
        }

        // Initialize the chart
        document.addEventListener('DOMContentLoaded', updateChart);
    </script>
</body>
</html>
