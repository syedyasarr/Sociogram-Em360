<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Team Sociogram Survey</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 10px;
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            overflow: hidden;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px 20px;
            text-align: center;
        }

        .header h1 {
            font-size: 1.8em;
            margin-bottom: 10px;
        }

        .header p {
            opacity: 0.9;
        }

        .content {
            padding: 25px;
        }

        .screen {
            display: none;
        }

        .screen.active {
            display: block;
            animation: fadeIn 0.3s;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* Admin Access */
        .admin-trigger {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 50px;
            height: 50px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-size: 1.5em;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
            z-index: 1000;
        }

        .admin-trigger:active {
            transform: scale(0.95);
        }

        /* Person Selection */
        .info-box {
            background: #e3f2fd;
            padding: 15px;
            border-radius: 12px;
            margin-bottom: 20px;
            border-left: 4px solid #2196F3;
        }

        .person-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
            margin-bottom: 20px;
        }

        .person-card {
            background: white;
            border: 3px solid #e0e0e0;
            border-radius: 15px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
        }

        .person-card:active {
            transform: scale(0.95);
        }

        .person-card.selected {
            border-color: #667eea;
            background: linear-gradient(135deg, #e8eaf6 0%, #f3e5f5 100%);
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.3);
        }

        .person-card.completed::after {
            content: '✓';
            position: absolute;
            top: 5px;
            right: 10px;
            background: #28a745;
            color: white;
            width: 25px;
            height: 25px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 0.9em;
        }

        .person-name {
            font-weight: 600;
            font-size: 1.1em;
            margin-bottom: 5px;
            color: #333;
        }

        .person-role {
            font-size: 0.8em;
            color: #666;
            text-transform: uppercase;
        }

        /* Survey Questions */
        .survey-header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            margin-bottom: 25px;
        }

        .survey-header h2 {
            font-size: 1.3em;
            margin-bottom: 5px;
        }

        .survey-header .role-tag {
            background: rgba(255,255,255,0.2);
            padding: 5px 12px;
            border-radius: 15px;
            font-size: 0.85em;
            display: inline-block;
            margin-top: 5px;
        }

        .progress {
            text-align: center;
            color: #666;
            margin-bottom: 15px;
            font-weight: 600;
        }

        .progress-bar {
            width: 100%;
            height: 8px;
            background: #e0e0e0;
            border-radius: 10px;
            overflow: hidden;
            margin-bottom: 20px;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            transition: width 0.4s ease;
        }

        .question-card {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
        }

        .question-title {
            font-size: 1.3em;
            font-weight: 600;
            color: #333;
            margin-bottom: 8px;
        }

        .question-subtitle {
            color: #666;
            margin-bottom: 20px;
            font-size: 0.95em;
        }

        .select-all {
            background: #e8eaf6;
            color: #667eea;
            padding: 12px;
            border: 2px solid #667eea;
            border-radius: 10px;
            width: 100%;
            font-weight: 600;
            cursor: pointer;
            margin-bottom: 15px;
            transition: all 0.3s;
        }

        .select-all:active {
            transform: scale(0.98);
        }

        .options-list {
            display: flex;
            flex-direction: column;
            gap: 10px;
            max-height: 400px;
            overflow-y: auto;
        }

        .option {
            background: white;
            padding: 15px;
            border-radius: 12px;
            border: 2px solid #e0e0e0;
            display: flex;
            align-items: center;
            gap: 12px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .option:active {
            transform: scale(0.98);
        }

        .option.selected {
            border-color: #667eea;
            background: #e8eaf6;
        }

        .checkbox {
            width: 26px;
            height: 26px;
            border: 3px solid #667eea;
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
            background: white;
            transition: all 0.3s;
        }

        .option.selected .checkbox {
            background: #667eea;
        }

        .checkmark {
            color: white;
            font-weight: bold;
            font-size: 1.2em;
            display: none;
        }

        .option.selected .checkmark {
            display: block;
        }

        .option-label {
            flex: 1;
            font-weight: 500;
            color: #333;
        }

        /* Buttons */
        .btn {
            width: 100%;
            padding: 18px;
            border: none;
            border-radius: 12px;
            font-size: 1.1em;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
            margin-top: 10px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            box-shadow: 0 4px 15px rgba(102, 126, 234, 0.4);
        }

        .btn-primary:active {
            transform: scale(0.98);
        }

        .btn-secondary {
            background: #6c757d;
            color: white;
        }

        /* Thank You Screen */
        .thank-you {
            text-align: center;
            padding: 40px 20px;
        }

        .thank-you-icon {
            font-size: 6em;
            margin-bottom: 20px;
            animation: bounce 0.6s;
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        .thank-you h2 {
            color: #333;
            margin-bottom: 15px;
            font-size: 1.8em;
        }

        .thank-you p {
            color: #666;
            line-height: 1.6;
        }

        /* Admin Dashboard */
        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 25px;
        }

        .stat-card {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 20px;
            border-radius: 15px;
            text-align: center;
        }

        .stat-card h3 {
            font-size: 0.85em;
            opacity: 0.9;
            margin-bottom: 8px;
        }

        .stat-value {
            font-size: 2.2em;
            font-weight: bold;
        }

        .action-section {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 20px;
        }

        .action-section h3 {
            color: #667eea;
            margin-bottom: 15px;
        }

        .response-item {
            background: white;
            padding: 15px;
            margin: 8px 0;
            border-radius: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-left: 4px solid #ccc;
        }

        .response-item.completed {
            border-left-color: #28a745;
        }

        .response-name {
            font-weight: 600;
        }

        .response-details {
            font-size: 0.85em;
            color: #666;
            margin-top: 3px;
        }

        .badge {
            padding: 5px 12px;
            border-radius: 15px;
            font-size: 0.8em;
            font-weight: 600;
        }

        .badge-success {
            background: #28a745;
            color: white;
        }

        .badge-warning {
            background: #ffc107;
            color: #333;
        }

        /* Canvas */
        #canvas {
            width: 100%;
            height: 500px;
            border: 2px solid #e0e0e0;
            border-radius: 12px;
            background: #fafafa;
            margin: 15px 0;
            touch-action: none;
        }

        .legend {
            background: #f8f9fa;
            padding: 15px;
            border-radius: 10px;
            margin-bottom: 15px;
            font-size: 0.9em;
        }

        .legend-item {
            display: flex;
            align-items: center;
            gap: 10px;
            margin: 8px 0;
        }

        .legend-color {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            border: 2px solid #333;
        }

        .legend-line {
            width: 30px;
            height: 3px;
        }

        .filter-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-bottom: 15px;
        }

        .filter-btn {
            padding: 12px;
            border: 2px solid #667eea;
            background: white;
            color: #667eea;
            border-radius: 10px;
            font-weight: 600;
            cursor: pointer;
        }

        .filter-btn.active {
            background: #667eea;
            color: white;
        }

        .analysis-card {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 15px;
            border-left: 4px solid #667eea;
        }

        .analysis-card h4 {
            color: #667eea;
            margin-bottom: 15px;
        }

        .rank-item {
            background: white;
            padding: 12px;
            margin: 8px 0;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
        }

        .loading {
            text-align: center;
            padding: 40px;
            color: #666;
        }

        .spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #667eea;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin: 20px auto;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🌐 Team Sociogram</h1>
            <p>Help us understand team dynamics</p>
        </div>

        <div class="content">
            <!-- Person Selection Screen -->
            <div id="select-screen" class="screen active">
                <div class="info-box">
                    <strong>👋 Welcome!</strong><br>
                    Select your name below to start the survey. This will help us understand our team relationships better.
                </div>
                <div class="person-grid" id="person-grid"></div>
            </div>

            <!-- Survey Screen -->
            <div id="survey-screen" class="screen">
                <div class="survey-header">
                    <h2 id="survey-name"></h2>
                    <div class="role-tag" id="survey-role"></div>
                </div>

                <div class="progress" id="progress-text">Question 1 of 3</div>
                <div class="progress-bar">
                    <div class="progress-fill" id="progress-fill"></div>
                </div>

                <div id="question-container"></div>

                <button class="btn btn-primary" id="next-btn" onclick="nextQuestion()">Next →</button>
            </div>

            <!-- Thank You Screen -->
            <div id="thanks-screen" class="screen">
                <div class="thank-you">
                    <div class="thank-you-icon">🎉</div>
                    <h2>Thank You!</h2>
                    <p>Your responses have been saved successfully.</p>
                    <p style="margin-top: 15px;">You can close this page now or fill another survey.</p>
                </div>
                <button class="btn btn-secondary" onclick="resetSurvey()">Fill Another Survey</button>
            </div>

            <!-- Admin Dashboard -->
            <div id="admin-screen" class="screen">
                <h2 style="margin-bottom: 20px; color: #667eea;">📊 Admin Dashboard</h2>
                
                <div class="stats-grid">
                    <div class="stat-card">
                        <h3>Responses</h3>
                        <div class="stat-value" id="stat-responses">0/20</div>
                    </div>
                    <div class="stat-card">
                        <h3>Complete</h3>
                        <div class="stat-value" id="stat-percent">0%</div>
                    </div>
                    <div class="stat-card">
                        <h3>Connections</h3>
                        <div class="stat-value" id="stat-connections">0</div>
                    </div>
                    <div class="stat-card">
                        <h3>Avg/Person</h3>
                        <div class="stat-value" id="stat-avg">0</div>
                    </div>
                </div>

                <div class="action-section">
                    <h3>🎯 Actions</h3>
                    <button class="btn btn-primary" onclick="showVisualization()">✨ View Sociogram</button>
                    <button class="btn btn-primary" onclick="showAnalysis()">📊 View Analysis</button>
                    <button class="btn btn-secondary" onclick="refreshAdmin()">🔄 Refresh</button>
                    <button class="btn btn-secondary" onclick="exportData()">💾 Export Data</button>
                </div>

                <div class="action-section">
                    <h3>👥 Responses (<span id="response-count">0</span>/20)</h3>
                    <div id="response-list"></div>
                </div>

                <button class="btn btn-secondary" onclick="closeAdmin()">← Back to Survey</button>
            </div>

            <!-- Visualization Screen -->
            <div id="viz-screen" class="screen">
                <h2 style="margin-bottom: 15px; color: #667eea;">🔮 Network Visualization</h2>
                
                <div class="legend">
                    <strong>Legend:</strong>
                    <div class="legend-item">
                        <div class="legend-color" style="background: #FFD700;"></div>
                        <span>CEO</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background: #4CAF50;"></div>
                        <span>Supervisor</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-color" style="background: #2196F3;"></div>
                        <span>Intern</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-line" style="background: #e91e63;"></div>
                        <span>Work</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-line" style="background: #9c27b0;"></div>
                        <span>Friendship</span>
                    </div>
                    <div class="legend-item">
                        <div class="legend-line" style="background: #ff9800;"></div>
                        <span>Communication</span>
                    </div>
                </div>

                <div class="filter-grid">
                    <button class="filter-btn active" onclick="filterViz('all')">🌟 All</button>
                    <button class="filter-btn" onclick="filterViz('work')">💼 Work</button>
                    <button class="filter-btn" onclick="filterViz('friend')">👥 Friends</button>
                    <button class="filter-btn" onclick="filterViz('communication')">💬 Comm</button>
                </div>

                <canvas id="canvas"></canvas>

                <button class="btn btn-primary" onclick="saveImage()">📸 Save Image</button>
                <button class="btn btn-secondary" onclick="backToAdmin()">← Back</button>
            </div>

            <!-- Analysis Screen -->
            <div id="analysis-screen" class="screen">
                <h2 style="margin-bottom: 15px; color: #667eea;">📊 Network Analysis</h2>
                <div id="analysis-content"></div>
                <button class="btn btn-secondary" onclick="backToAdmin()">← Back</button>
            </div>
        </div>
    </div>

    <!-- Admin Trigger Button -->
    <div class="admin-trigger" onclick="toggleAdmin()">⚙️</div>

    <script>
        const colleagues = [
            { id: 1, name: "Yasar", role: "Intern" },
            { id: 2, name: "Nandani", role: "Intern" },
            { id: 3, name: "Vrinda", role: "Intern" },
            { id: 4, name: "Prapti", role: "Intern" },
            { id: 5, name: "Pradipta", role: "Intern" },
            { id: 6, name: "Devyani", role: "Intern" },
            { id: 7, name: "Lakshyata", role: "Intern" },
            { id: 8, name: "Stuti", role: "Intern" },
            { id: 9, name: "Hershi", role: "Intern" },
            { id: 10, name: "Hinal", role: "Intern" },
            { id: 11, name: "Shraddha", role: "Intern" },
            { id: 12, name: "Jenil", role: "Supervisor" },
            { id: 13, name: "Pooja", role: "Intern" },
            { id: 14, name: "Ayushi", role: "Intern" },
            { id: 15, name: "Deeya", role: "Intern" },
            { id: 16, name: "Grisha", role: "Intern" },
            { id: 17, name: "Niyati", role: "Supervisor" },
            { id: 18, name: "Ishika", role: "Intern" },
            { id: 19, name: "Krishna", role: "Intern" },
            { id: 20, name: "Kaeyoor Joshi", role: "CEO" }
        ];

        const questions = [
            { id: 'work', icon: '💼', title: 'Work Collaboration', subtitle: 'Select people you prefer to work with on projects' },
            { id: 'friend', icon: '👥', title: 'Friendships', subtitle: 'Select people you consider friends or feel close to' },
            { id: 'communication', icon: '💬', title: 'Communication', subtitle: 'Select people you communicate with frequently' }
        ];

        let currentPerson = null;
        let currentQuestionIndex = 0;
        let surveyData = { work: [], friend: [], communication: [] };
        let allResponses = {};
        let completedIds = [];
        let nodes = [];
        let currentFilter = 'all';
        const canvas = document.getElementById('canvas');
        const ctx = canvas?.getContext('2d');

        // Initialize
        async function init() {
            await loadAllData();
            renderPersonGrid();
        }

        async function loadAllData() {
            try {
                const listResult = await window.storage.get('response_list', true);
                completedIds = listResult ? JSON.parse(listResult.value) : [];
            } catch (e) {
                completedIds = [];
            }

            allResponses = {};
            for (const id of completedIds) {
                try {
                    const result = await window.storage.get(`response_${id}`, true);
                    if (result) {
                        allResponses[id] = JSON.parse(result.value);
                    }
                } catch (e) {}
            }
        }

        function renderPersonGrid() {
            const grid = document.getElementById('person-grid');
            grid.innerHTML = colleagues.map(person => `
                <div class="person-card ${completedIds.includes(person.id) ? 'completed' : ''}" 
                     onclick="selectPerson(${person.id})">
                    <div class="person-name">${person.name}</div>
                    <div class="person-role">${person.role}</div>
                </div>
            `).join('');
        }

        async function selectPerson(id) {
            currentPerson = colleagues.find(c => c.id === id);
            currentQuestionIndex = 0;
            surveyData = { work: [], friend: [], communication: [] };

            // Load existing data
            try {
                const result = await window.storage.get(`response_${id}`, true);
                if (result) {
                    const data = JSON.parse(result.value);
                    surveyData = {
                        work: data.work || [],
                        friend: data.friend || [],
                        communication: data.communication || []
                    };
                }
            } catch (e) {}

            showScreen('survey-screen');
            document.getElementById('survey-name').textContent = currentPerson.name;
            document.getElementById('survey-role').textContent = currentPerson.role;
            showQuestion();
        }

        function showQuestion() {
            const question = questions[currentQuestionIndex];
            const others = colleagues.filter(c => c.id !== currentPerson.id);
            
            const progress = ((currentQuestionIndex + 1) / questions.length) * 100;
            document.getElementById('progress-fill').style.width = progress + '%';
            document.getElementById('progress-text').textContent = `Question ${currentQuestionIndex + 1} of ${questions.length}`;

            const container = document.getElementById('question-container');
            container.innerHTML = `
                <div class="question-card">
                    <div class="question-title">${question.icon} ${question.title}</div>
                    <div class="question-subtitle">${question.subtitle}</div>
                    <button class="select-all" onclick="toggleSelectAll('${question.id}')">
                        Select All / Deselect All
                    </button>
                    <div class="options-list" id="options-${question.id}">
                        ${others.map(p => `
                            <div class="option ${surveyData[question.id].includes(p.id) ? 'selected' : ''}" 
                                 data-id="${p.id}" onclick="toggleOption('${question.id}', ${p.id})">
                                <div class="checkbox">
                                    <span class="checkmark">✓</span>
                                </div>
                                <div class="option-label">${p.name}</div>
                            </div>
                        `).join('')}
                    </div>
                </div>
            `;

            const nextBtn = document.getElementById('next-btn');
            nextBtn.textContent = currentQuestionIndex === questions.length - 1 ? 'Submit ✓' : 'Next →';
        }

        function toggleOption(questionId, personId) {
            const option = document.querySelector(`#options-${questionId} .option[data-id="${personId}"]`);
            option.classList.toggle('selected');
            
            const index = surveyData[questionId].indexOf(personId);
            if (index > -1) {
                surveyData[questionId].splice(index, 1);
            } else {
                surveyData[questionId].push(personId);
            }
        }

        function toggleSelectAll(questionId) {
            const options = document.querySelectorAll(`#options-${questionId} .option`);
            const allSelected = Array.from(options).every(o => o.classList.contains('selected'));
            
            options.forEach(opt => {
                const personId = parseInt(opt.dataset.id);
                if (allSelected) {
                    opt.classList.remove('selected');
                    surveyData[questionId] = [];
                } else {
                    opt.classList.add('selected');
                    if (!surveyData[questionId].includes(personId)) {
                        surveyData[questionId].push(personId);
                    }
                }
            });
        }

        function nextQuestion() {
            if (currentQuestionIndex < questions.length - 1) {
                currentQuestionIndex++;
                showQuestion();
            } else {
                submitSurvey();
            }
        }

        async function submitSurvey() {
            const data = {
                personId: currentPerson.id,
                personName: currentPerson.name,
                work: surveyData.work,
                friend: surveyData.friend,
                communication: surveyData.communication,
                timestamp: new Date().toISOString()
            };

            try {
                await window.storage.set(`response_${currentPerson.id}`, JSON.stringify(data), true);
                
                if (!completedIds.includes(currentPerson.id)) {
                    completedIds.push(currentPerson.id);
                    await window.storage.set('response_list', JSON.stringify(completedIds), true);
                }
                
                allResponses[currentPerson.id] = data;
                showScreen('thanks-screen');
            } catch (error) {
                alert('Error saving response. Please try again.');
                console.error(error);
            }
        }

        function resetSurvey() {
            currentPerson = null;
            renderPersonGrid();
            showScreen('select-screen');
        }

        // Admin Functions
        function toggleAdmin() {
            if (document.getElementById('admin-screen').classList.contains('active')) {
                showScreen('select-screen');
            } else {
                loadAdminData();
                showScreen('admin-screen');
            }
        }

        async function refreshAdmin() {
            await loadAllData();
            loadAdminData();
        }

        function loadAdminData() {
            const responseCount = Object.keys(allResponses).length;
            const percent = Math.round((responseCount / colleagues.length) * 100);
            
            let totalConnections = 0;
            Object.values(allResponses).forEach(r => {
                totalConnections += (r.work?.length || 0) + (r.friend?.length || 0) + (r.communication?.length || 0);
            });
            
            const avg = responseCount > 0 ? (totalConnections / responseCount).toFixed(1) : 0;

            document.getElementById('stat-responses').textContent = `${responseCount}/20`;
            document.getElementById('stat-percent').textContent = `${percent}%`;
            document.getElementById('stat-connections').textContent = totalConnections;
            document.getElementById('stat-avg').textContent = avg;
            document.getElementById('response-count').textContent = responseCount;

            const list = document.getElementById('response-list');
            list.innerHTML = colleagues.map(p => {
                const hasResponse = allResponses[p.id];
                const badge = hasResponse ? 
                    '<span class="badge badge-success">✓ Done</span>' : 
                    '<span class="badge badge-warning">Pending</span>';
                
                let details = '';
                if (hasResponse) {
                    const r = allResponses[p.id];
                    details = `<div class="response-details">W:${r.work?.length||0} F:${r.friend?.length||0} C:${r.communication?.length||0}</div>`;
                }
                
                return `
                    <div class="response-item ${hasResponse ? 'completed' : ''}">
                        <div>
                            <div class="response-name">${p.name}</div>
                            ${details}
                        </div>
                        ${badge}
                    </div>
                `;
            }).join('');
        }

        function closeAdmin() {
            showScreen('select-screen');
        }

        function exportData() {
            const data = {
                colleagues,
                responses: allResponses,
                exportDate: new Date().toISOString()
            };
            
            const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'sociogram-data.json';
            a.click();
            URL.revokeObjectURL(url);
        }

        // Visualization
        function showVisualization() {
            if (Object.keys(allResponses).length === 0) {
                alert('No responses yet!');
                return;
            }
            
            initNodes();
            applyForces();
            showScreen('viz-screen');
            setTimeout(() => {
                resizeCanvas();
                drawGraph();
            }, 100);
        }

        function resizeCanvas() {
            const rect = canvas.getBoundingClientRect();
            canvas.width = rect.width;
            canvas.height = rect.height;
        }

        function initNodes() {
            nodes = colleagues.map((p, i) => {
                const angle = (i / colleagues.length) * 2 * Math.PI;
                const radius = 150;
                return {
                    ...p,
                    x: 200 + radius * Math.cos(angle),
                    y: 200 + radius * Math.sin(angle),
                    vx: 0,
                    vy: 0
                };
            });
        }

        function applyForces() {
            for (let iter = 0; iter < 100; iter++) {
                nodes.forEach(n => { n.fx = 0; n.fy = 0; });
                
                // Repulsion
                for (let i = 0; i < nodes.length; i++) {
                    for (let j = i + 1; j < nodes.length; j++) {
                        const dx = nodes[j].x - nodes[i].x;
                        const dy = nodes[j].y - nodes[i].y;
                        const dist = Math.sqrt(dx*dx + dy*dy) || 1;
                        const force = 2000 / (dist * dist);
                        
                        nodes[i].fx -= force * dx / dist;
                        nodes[i].fy -= force * dy / dist;
                        nodes[j].fx += force * dx / dist;
                        nodes[j].fy += force * dy / dist;
                    }
                }
                
                // Attraction
                nodes.forEach(node => {
                    const resp = allResponses[node.id];
                    if (resp) {
                        ['work', 'friend', 'communication'].forEach(type => {
                            (resp[type] || []).forEach(targetId => {
                                const target = nodes.find(n => n.id === targetId);
                                if (target) {
                                    const dx = target.x - node.x;
                                    const dy = target.y - node.y;
                                    const dist = Math.sqrt(dx*dx + dy*dy) || 1;
                                    const force = 0.03 * (dist - 80);
                                    
                                    node.fx += force * dx / dist;
                                    node.fy += force * dy / dist;
                                }
                            });
                        });
                    }
                });
                
                // Update positions
                nodes.forEach(node => {
                    node.vx = (node.vx + node.fx) * 0.85;
                    node.vy = (node.vy + node.fy) * 0.85;
                    node.x += node.vx;
                    node.y += node.vy;
                    
                    node.x = Math.max(40, Math.min(360, node.x));
                    node.y = Math.max(40, Math.min(460, node.y));
                });
            }
        }

        function drawGraph() {
            if (!ctx) return;
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // Scale to canvas
            const scaleX = canvas.width / 400;
            const scaleY = canvas.height / 500;
            
            // Draw edges
            nodes.forEach(node => {
                const resp = allResponses[node.id];
                if (!resp) return;
                
                const types = currentFilter === 'all' ? ['work', 'friend', 'communication'] : [currentFilter];
                types.forEach(type => {
                    (resp[type] || []).forEach(targetId => {
                        const target = nodes.find(n => n.id === targetId);
                        if (target) {
                            const targetResp = allResponses[targetId];
                            const mutual = targetResp && targetResp[type] && targetResp[type].includes(node.id);
                            
                            const colors = { work: '#e91e63', friend: '#9c27b0', communication: '#ff9800' };
                            ctx.strokeStyle = colors[type];
                            ctx.lineWidth = mutual ? 3 : 1.5;
                            ctx.globalAlpha = 0.6;
                            
                            ctx.beginPath();
                            ctx.moveTo(node.x * scaleX, node.y * scaleY);
                            ctx.lineTo(target.x * scaleX, target.y * scaleY);
                            ctx.stroke();
                            
                            if (!mutual) {
                                const angle = Math.atan2((target.y - node.y) * scaleY, (target.x - node.x) * scaleX);
                                const tipX = target.x * scaleX - Math.cos(angle) * 20;
                                const tipY = target.y * scaleY - Math.sin(angle) * 20;
                                
                                ctx.beginPath();
                                ctx.moveTo(tipX, tipY);
                                ctx.lineTo(tipX - 8 * Math.cos(angle - Math.PI/6), tipY - 8 * Math.sin(angle - Math.PI/6));
                                ctx.moveTo(tipX, tipY);
                                ctx.lineTo(tipX - 8 * Math.cos(angle + Math.PI/6), tipY - 8 * Math.sin(angle + Math.PI/6));
                                ctx.stroke();
                            }
                            ctx.globalAlpha = 1;
                        }
                    });
                });
            });
            
            // Draw nodes
            nodes.forEach(node => {
                const colors = { CEO: '#FFD700', Supervisor: '#4CAF50', Intern: '#2196F3' };
                const hasResp = allResponses[node.id];
                
                ctx.fillStyle = colors[node.role];
                ctx.strokeStyle = '#333';
                ctx.lineWidth = 2;
                ctx.globalAlpha = hasResp ? 1 : 0.3;
                
                ctx.beginPath();
                ctx.arc(node.x * scaleX, node.y * scaleY, 12, 0, 2 * Math.PI);
                ctx.fill();
                ctx.stroke();
                
                ctx.globalAlpha = 1;
                ctx.fillStyle = '#333';
                ctx.font = 'bold 9px Arial';
                ctx.textAlign = 'center';
                ctx.fillText(node.name, node.x * scaleX, node.y * scaleY - 18);
            });
        }

        function filterViz(type) {
            currentFilter = type;
            document.querySelectorAll('.filter-btn').forEach(b => b.classList.remove('active'));
            event.target.classList.add('active');
            drawGraph();
        }

        function saveImage() {
            const a = document.createElement('a');
            a.download = 'sociogram.png';
            a.href = canvas.toDataURL();
            a.click();
        }

        // Analysis
        function showAnalysis() {
            if (Object.keys(allResponses).length === 0) {
                alert('No responses yet!');
                return;
            }
            
            generateAnalysis();
            showScreen('analysis-screen');
        }

        function generateAnalysis() {
            const counts = {};
            colleagues.forEach(p => counts[p.id] = { in: 0, out: 0 });
            
            let total = 0, mutual = 0;
            Object.entries(allResponses).forEach(([id, resp]) => {
                id = parseInt(id);
                ['work', 'friend', 'communication'].forEach(type => {
                    (resp[type] || []).forEach(targetId => {
                        total++;
                        counts[id].out++;
                        counts[targetId].in++;
                        
                        const targetResp = allResponses[targetId];
                        if (targetResp && targetResp[type] && targetResp[type].includes(id)) {
                            mutual++;
                        }
                    });
                });
            });
            mutual /= 2;
            
            const popular = colleagues
                .map(p => ({ name: p.name, count: counts[p.id].in }))
                .sort((a, b) => b.count - a.count)
                .slice(0, 5);
            
            const connected = colleagues
                .map(p => ({ name: p.name, count: counts[p.id].in + counts[p.id].out }))
                .sort((a, b) => b.count - a.count)
                .slice(0, 5);

            const responseCount = Object.keys(allResponses).length;
            
            document.getElementById('analysis-content').innerHTML = `
                <div class="analysis-card">
                    <h4>📊 Overview</h4>
                    <div style="font-size: 1.5em; font-weight: bold; margin: 10px 0;">${total} Connections</div>
                    <div>✓ Mutual: ${mutual} | → One-way: ${total - mutual*2}</div>
                    <div>📈 Reciprocity: ${((mutual*2/total)*100).toFixed(0)}%</div>
                </div>

                <div class="analysis-card">
                    <h4>🌟 Most Popular</h4>
                    ${popular.map((p, i) => `
                        <div class="rank-item">
                            <span>${i+1}. ${p.name}</span>
                            <strong>${p.count}</strong>
                        </div>
                    `).join('')}
                </div>

                <div class="analysis-card">
                    <h4>🔗 Most Connected</h4>
                    ${connected.map((p, i) => `
                        <div class="rank-item">
                            <span>${i+1}. ${p.name}</span>
                            <strong>${p.count}</strong>
                        </div>
                    `).join('')}
                </div>

                <div class="info-box">
                    <strong>💡 Insights</strong><br>
                    • Response rate: ${responseCount}/20 (${(responseCount/20*100).toFixed(0)}%)<br>
                    • Avg connections: ${(total/responseCount).toFixed(1)} per person<br>
                    • Network density: ${(total/(responseCount*19)*100).toFixed(1)}%
                </div>
            `;
        }

        function backToAdmin() {
            showScreen('admin-screen');
        }

        // Utility
        function showScreen(screenId) {
            document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
            document.getElementById(screenId).classList.add('active');
        }

        // Touch support for canvas
        if (canvas) {
            canvas.addEventListener('touchmove', (e) => {
                e.preventDefault();
                const touch = e.touches[0];
                const rect = canvas.getBoundingClientRect();
                const x = touch.clientX - rect.left;
                const y = touch.clientY - rect.top;
                
                const scaleX = 400 / canvas.width;
                const scaleY = 500 / canvas.height;
                const node = nodes.find(n => {
                    const dx = x - n.x / scaleX;
                    const dy = y - n.y / scaleY;
                    return Math.sqrt(dx*dx + dy*dy) < 15;
                });
                
                if (node) {
                    node.x = x * scaleX;
                    node.y = y * scaleY;
                    drawGraph();
                }
            });
        }

        // Initialize on load
        init();
    </script>
</body>
</html>
