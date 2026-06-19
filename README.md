<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ms Hien - IELTS Speaking Grader</title>
    <style>
        :root {
            --primary: #2563eb;
            --bg: #f8fafc;
            --panel: #ffffff;
            --text: #1e293b;
            --border: #e2e8f0;
            --danger: #ef4444;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg);
            color: var(--text);
            margin: 0;
            padding: 20px;
            line-height: 1.6;
        }
        .header {
            text-align: center;
            margin-bottom: 30px;
        }
        .header h1 { margin: 0; color: var(--primary); }
        .container {
            display: flex;
            gap: 20px;
            max-width: 1200px;
            margin: 0 auto;
        }
        @media (max-width: 768px) {
            .container { flex-direction: column; }
        }
        .panel {
            background: var(--panel);
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
            flex: 1;
            border: 1px solid var(--border);
        }
        .panel-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px solid var(--border);
            padding-bottom: 10px;
            margin-bottom: 15px;
        }
        h2 { margin: 0; font-size: 1.2rem; }
        
        /* Audio Player & Feedback */
        .audio-section { margin-bottom: 20px; }
        input[type="file"] { margin-bottom: 10px; }
        audio { width: 100%; margin-top: 10px; outline: none; }
        .feedback-input {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        input[type="text"] {
            flex: 1;
            padding: 10px;
            border: 1px solid var(--border);
            border-radius: 6px;
        }
        button {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
        }
        button:hover { background-color: #1d4ed8; }
        
        .btn-danger {
            background-color: var(--danger);
            padding: 6px 12px;
            font-size: 0.9rem;
        }
        .btn-danger:hover { background-color: #dc2626; }

        .comment-list { list-style: none; padding: 0; margin: 0; }
        .comment-item {
            background: var(--bg);
            padding: 12px;
            border-radius: 6px;
            margin-bottom: 8px;
            border-left: 4px solid var(--primary);
            display: flex;
            gap: 15px;
            align-items: center;
        }
        .timestamp-badge {
            background: var(--primary);
            color: white;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 0.85rem;
            cursor: pointer;
        }
        
        /* Rubric */
        .criteria-group { margin-bottom: 15px; }
        .criteria-group label {
            display: block;
            font-weight: 600;
            margin-bottom: 5px;
        }
        select {
            width: 100%;
            padding: 10px;
            border: 1px solid var(--border);
            border-radius: 6px;
            background-color: var(--bg);
        }
        .score-display {
            background: var(--primary);
            color: white;
            padding: 20px;
            text-align: center;
            border-radius: 8px;
            margin-top: 20px;
        }
        .score-display h3 { margin: 0; font-size: 1.5rem; }
        .score-display .final-score { font-size: 3rem; font-weight: bold; margin-top: 10px; }
    </style>
</head>
<body>

    <div class="header">
        <h1>Ms Hien</h1>
        <p>IELTS Speaking Interactive Grader</p>
    </div>

    <div class="container">
        <!-- Left Panel: Audio and Feedback -->
        <div class="panel">
            <div class="panel-header">
                <h2>1. Workspace & Audio</h2>
                <button class="btn-danger" onclick="clearWorkspace()">Clear Workspace</button>
            </div>
            
            <div class="audio-section">
                <input type="file" id="audioUpload" accept="audio/*">
                <audio id="audioPlayer" controls></audio>
            </div>

            <div class="panel-header" style="margin-top: 20px;">
                <h2>2. Timestamped Feedback</h2>
            </div>
            
            <div class="feedback-input">
                <input type="text" id="commentInput" placeholder="E.g., Good vocabulary here, but watch pronunciation...">
                <button onclick="addComment()">Add Note</button>
            </div>
            <ul class="comment-list" id="commentList">
                <!-- Comments will appear here -->
            </ul>
        </div>

        <!-- Right Panel: Rubric -->
        <div class="panel">
            <div class="panel-header">
                <h2>3. IELTS Scoring Rubric</h2>
            </div>
            
            <div class="criteria-group">
                <label>Fluency and Coherence (FC)</label>
                <select id="score-fc" onchange="calculateScore()">
                    <option value="0">Select Band</option>
                    <option value="9">Band 9.0</option>
                    <option value="8">Band 8.0</option>
                    <option value="7">Band 7.0</option>
                    <option value="6">Band 6.0</option>
                    <option value="5">Band 5.0</option>
                    <option value="4">Band 4.0</option>
                </select>
            </div>

            <div class="criteria-group">
                <label>Lexical Resource (LR)</label>
                <select id="score-lr" onchange="calculateScore()">
                    <option value="0">Select Band</option>
                    <option value="9">Band 9.0</option>
                    <option value="8">Band 8.0</option>
                    <option value="7">Band 7.0</option>
                    <option value="6">Band 6.0</option>
                    <option value="5">Band 5.0</option>
                    <option value="4">Band 4.0</option>
                </select>
            </div>

            <div class="criteria-group">
                <label>Grammatical Range and Accuracy (GRA)</label>
                <select id="score-gra" onchange="calculateScore()">
                    <option value="0">Select Band</option>
                    <option value="9">Band 9.0</option>
                    <option value="8">Band 8.0</option>
                    <option value="7">Band 7.0</option>
                    <option value="6">Band 6.0</option>
                    <option value="5">Band 5.0</option>
                    <option value="4">Band 4.0</option>
                </select>
            </div>

            <div class="criteria-group">
                <label>Pronunciation (PR)</label>
                <select id="score-pr" onchange="calculateScore()">
                    <option value="0">Select Band</option>
                    <option value="9">Band 9.0</option>
                    <option value="8">Band 8.0</option>
                    <option value="7">Band 7.0</option>
                    <option value="6">Band 6.0</option>
                    <option value="5">Band 5.0</option>
                    <option value="4">Band 4.0</option>
                </select>
            </div>

            <div class="score-display">
                <h3>Overall Band Score</h3>
                <div class="final-score" id="finalScore">0.0</div>
            </div>
            
            <button style="width: 100%; margin-top: 20px; background-color: #10b981;" onclick="exportReport()">Export Feedback Report</button>
        </div>
    </div>

    <script>
        const audioUpload = document.getElementById('audioUpload');
        const audioPlayer = document.getElementById('audioPlayer');
        const commentInput = document.getElementById('commentInput');
        const commentList = document.getElementById('commentList');

        // Load saved data when the page loads
        window.onload = function() {
            if(localStorage.getItem('ielts_comments')) {
                commentList.innerHTML = localStorage.getItem('ielts_comments');
            }
            if(localStorage.getItem('ielts_fc')) {
                document.getElementById('score-fc').value = localStorage.getItem('ielts_fc');
                document.getElementById('score-lr').value = localStorage.getItem('ielts_lr');
                document.getElementById('score-gra').value = localStorage.getItem('ielts_gra');
                document.getElementById('score-pr').value = localStorage.getItem('ielts_pr');
                document.getElementById('finalScore').innerText = localStorage.getItem('ielts_final') || '0.0';
            }
        };

        // Save data to local storage
        function saveDataLocally() {
            localStorage.setItem('ielts_comments', commentList.innerHTML);
            localStorage.setItem('ielts_fc', document.getElementById('score-fc').value);
            localStorage.setItem('ielts_lr', document.getElementById('score-lr').value);
            localStorage.setItem('ielts_gra', document.getElementById('score-gra').value);
            localStorage.setItem('ielts_pr', document.getElementById('score-pr').value);
            localStorage.setItem('ielts_final', document.getElementById('finalScore').innerText);
        }

        // Clear workspace
        function clearWorkspace() {
            if(confirm("Are you sure you want to clear all notes and scores? This cannot be undone.")) {
                localStorage.clear();
                commentList.innerHTML = '';
                document.getElementById('score-fc').value = '0';
                document.getElementById('score-lr').value = '0';
                document.getElementById('score-gra').value = '0';
                document.getElementById('score-pr').value = '0';
                document.getElementById('finalScore').innerText = '0.0';
                audioUpload.value = '';
                audioPlayer.src = '';
            }
        }

        // Load audio file into player
        audioUpload.addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (file) {
                const url = URL.createObjectURL(file);
                audioPlayer.src = url;
            }
        });

        // Format time in MM:SS
        function formatTime(seconds) {
            const m = Math.floor(seconds / 60);
            const s = Math.floor(seconds % 60);
            return `${m}:${s < 10 ? '0' : ''}${s}`;
        }

        // Add a timestamped comment
        function addComment() {
            const text = commentInput.value.trim();
            if (!text) return;

            const timeInSeconds = audioPlayer.currentTime || 0;
            const timeFormatted = formatTime(timeInSeconds);

            const li = document.createElement('li');
            li.className = 'comment-item';
            
            li.innerHTML = `
                <span class="timestamp-badge" onclick="playAt(${timeInSeconds})" title="Click to play from here">
                    ⏱ ${timeFormatted}
                </span>
                <span>${text}</span>
            `;
            
            commentList.appendChild(li);
            commentInput.value = ''; 
            
            saveDataLocally(); // Save immediately after adding
        }

        // Allow pressing "Enter" to add note
        commentInput.addEventListener("keypress", function(event) {
            if (event.key === "Enter") {
                event.preventDefault();
                addComment();
            }
        });

        // Click a timestamp to jump audio to that point
        window.playAt = function(seconds) {
            audioPlayer.currentTime = seconds;
            audioPlayer.play();
        }

        // Calculate official IELTS average (round to nearest 0.5)
        function calculateScore() {
            const fc = parseFloat(document.getElementById('score-fc').value);
            const lr = parseFloat(document.getElementById('score-lr').value);
            const gra = parseFloat(document.getElementById('score-gra').value);
            const pr = parseFloat(document.getElementById('score-pr').value);

            if (fc === 0 || lr === 0 || gra === 0 || pr === 0) {
                document.getElementById('finalScore').innerText = "-";
                saveDataLocally();
                return;
            }

            const average = (fc + lr + gra + pr) / 4;
            const final = Math.round(average * 2) / 2; 
            
            document.getElementById('finalScore').innerText = final.toFixed(1);
            saveDataLocally(); // Save immediately after changing score
        }

        // Export simple text report
        function exportReport() {
            const finalScore = document.getElementById('finalScore').innerText;
            if(finalScore === "-" || finalScore === "0.0") {
                alert("Please fill out all rubric scores first.");
                return;
            }

            let report = "IELTS SPEAKING FEEDBACK REPORT\n";
            report += "==============================\n";
            report += `Final Score: ${finalScore}\n\n`;
            
            report += "TIMESTAMPTED NOTES:\n";
            const comments = document.querySelectorAll('.comment-item');
            if(comments.length === 0) report += "No notes added.\n";
            
            comments.forEach(c => {
                const time = c.querySelector('.timestamp-badge').innerText.replace('⏱ ', '');
                const note = c.querySelector('span:nth-child(2)').innerText;
                report += `[${time}] ${note}\n`;
            });

            // Create downloadable text file
            const blob = new Blob([report], { type: 'text/plain' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = "Speaking_Feedback_Report.txt";
            a.click();
            URL.revokeObjectURL(url);
        }
    </script>
</body>
</html>
# Ms.Hien-IELTS-SPEAKING-RUBRIC
For marking ielts speaking
