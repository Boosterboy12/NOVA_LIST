<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NovaList | Space for Your Tasks</title>
    <style>
        /* --- DESIGN TOKENS & VARIABLES (Space Theme) --- */
        :root {
            --bg-deep-space: #0b0b14;
            --bg-card: #131324;
            --text-main: #f5f6fa;
            --text-muted: #a4b0be;
            --accent-glow: #6c5ce7;
            --accent-cyan: #00cec9;
            --accent-danger: #ff7675;
            --border-color: #232342;
            --font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        /* --- RESET & BASE STYLES --- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background-color: var(--bg-deep-space);
            color: var(--text-main);
            font-family: var(--font-family);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
            overflow-x: hidden;
        }

        /* Ambient Glow in Background */
        body::before {
            content: '';
            position: absolute;
            width: 300px;
            height: 300px;
            background: var(--accent-glow);
            filter: blur(150px);
            top: 10%;
            left: 20%;
            z-index: -1;
            opacity: 0.3;
        }

        body::after {
            content: '';
            position: absolute;
            width: 300px;
            height: 300px;
            background: var(--accent-cyan);
            filter: blur(150px);
            bottom: 10%;
            right: 20%;
            z-index: -1;
            opacity: 0.2;
        }

        /* --- MAIN APP CONTAINER --- */
        .novalist-container {
            background-color: var(--bg-card);
            width: 100%;
            max-width: 500px;
            border-radius: 16px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
            border: 1px solid var(--border-color);
            backdrop-filter: blur(10px);
        }

        /* --- HEADER --- */
        header {
            margin-bottom: 25px;
            text-align: center;
        }

        header h1 {
            font-size: 2.2rem;
            font-weight: 700;
            letter-spacing: 1px;
            background: linear-gradient(45deg, var(--text-main), var(--accent-cyan));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 5px;
        }

        header p {
            color: var(--text-muted);
            font-size: 0.9rem;
        }

        /* --- INPUT FIELD SECTION --- */
        .input-group {
            display: flex;
            gap: 10px;
            margin-bottom: 25px;
        }

        .input-group input {
            flex: 1;
            background-color: var(--bg-deep-space);
            border: 1px solid var(--border-color);
            padding: 14px 16px;
            border-radius: 8px;
            color: var(--text-main);
            font-size: 1rem;
            outline: none;
            transition: all 0.3s ease;
        }

        .input-group input:focus {
            border-color: var(--accent-glow);
            box-shadow: 0 0 8px rgba(108, 92, 231, 0.4);
        }

        .input-group button {
            background: linear-gradient(135deg, var(--accent-glow), #5b4bc4);
            color: white;
            border: none;
            padding: 0 20px;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .input-group button:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(108, 92, 231, 0.3);
        }

        /* --- TASK LIST DISPLAY --- */
        .task-list {
            list-style: none;
            max-height: 400px;
            overflow-y: auto;
            padding-right: 5px;
        }

        /* Custom Scrollbar for Task List */
        .task-list::-webkit-scrollbar {
            width: 6px;
        }
        .task-list::-webkit-scrollbar-track {
            background: var(--bg-deep-space);
        }
        .task-list::-webkit-scrollbar-thumb {
            background: var(--border-color);
            border-radius: 10px;
        }

        .task-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: var(--bg-deep-space);
            border: 1px solid var(--border-color);
            padding: 14px 16px;
            border-radius: 8px;
            margin-bottom: 12px;
            animation: fadeIn 0.3s ease-in-out;
            transition: all 0.2s ease;
        }

        .task-item:hover {
            border-color: #32325c;
            transform: scale(1.01);
        }

        .task-content {
            display: flex;
            align-items: center;
            gap: 12px;
            cursor: pointer;
            flex: 1;
        }

        /* Custom Checkbox Vibe */
        .checkbox-mock {
            width: 20px;
            height: 20px;
            border: 2px solid var(--accent-cyan);
            border-radius: 6px;
            display: inline-block;
            position: relative;
            transition: all 0.2s ease;
        }

        .task-item.completed .checkbox-mock {
            background-color: var(--accent-cyan);
            border-color: var(--accent-cyan);
        }

        .task-item.completed .checkbox-mock::after {
            content: '✓';
            position: absolute;
            color: var(--bg-deep-space);
            font-size: 12px;
            font-weight: bold;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }

        .task-text {
            font-size: 1rem;
            color: var(--text-main);
            transition: all 0.2s ease;
            user-select: none;
        }

        .task-item.completed .task-text {
            text-decoration: line-through;
            color: var(--text-muted);
            opacity: 0.6;
        }

        /* Delete Button Style */
        .btn-delete {
            background: transparent;
            border: none;
            color: var(--text-muted);
            font-size: 1.2rem;
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 4px;
            transition: all 0.2s ease;
        }

        .btn-delete:hover {
            color: var(--accent-danger);
            background-color: rgba(255, 118, 117, 0.1);
        }

        /* Empty State Hint */
        .empty-state {
            text-align: center;
            color: var(--text-muted);
            padding: 20px 0;
            font-size: 0.95rem;
            font-style: italic;
        }

        /* --- ANIMATIONS --- */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body>

    <div class="novalist-container">
        <!-- Header Section -->
        <header>
            <h1>NovaList</h1>
            <p>Organize your stellar routines efficiently</p>
        </header>

        <!-- Task Injection Controls -->
        <div class="input-group">
            <input type="text" id="taskInput" placeholder="Add a new tactical mission..." autocomplete="off">
            <button id="addTaskBtn">Launch</button>
        </div>

        <!-- Task Dynamic Output Wrapper -->
        <ul class="task-list" id="taskList">
            <!-- Dynamic list objects inject here via execution script -->
        </ul>
    </div>

    <!-- --- WEB APPLICATION CORE LOGICAL ENGINE --- -->
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const taskInput = document.getElementById('taskInput');
            const addTaskBtn = document.getElementById('addTaskBtn');
            const taskList = document.getElementById('taskList');

            // Initialize State Pipeline from client-side Storage
            let tasks = JSON.parse(localStorage.getItem('NOVA_LIST_STATE')) || [];

            // Core State Sync Pipeline
            const saveStateToStorage = () => {
                localStorage.setItem('NOVA_LIST_STATE', JSON.stringify(tasks));
            };

            // Complete UI Rendering Operations
            const renderTasks = () => {
                taskList.innerHTML = '';

                if (tasks.length === 0) {
                    taskList.innerHTML = '<div class="empty-state">No active missions in deep orbit. Add one above!</div>';
                    return;
                }

                tasks.forEach((task, index) => {
                    const li = document.createElement('li');
                    li.className = `task-item ${task.completed ? 'completed' : ''}`;

                    li.innerHTML = `
                        <div class="task-content" data-index="${index}">
                            <span class="checkbox-mock"></span>
                            <span class="task-text">${escapeHTML(task.text)}</span>
                        </div>
                        <button class="btn-delete" data-index="${index}">&times;</button>
                    `;

                    taskList.appendChild(li);
                });
            };

            // Secure Entity Inputs against malicious DOM manipulations
            const escapeHTML = (string) => {
                const div = document.createElement('div');
                div.innerText = string;
                return div.innerHTML;
            };

            // Task Creation Controller
            const createNewTask = () => {
                const taskText = taskInput.value.trim();
                if (taskText === '') return;

                tasks.push({
                    text: taskText,
                    completed: false
                });

                taskInput.value = '';
                saveStateToStorage();
                renderTasks();
            };

            // Mutation Event Handler mapping delegation
            taskList.addEventListener('click', (e) => {
                // Toggle complete status processing
                const contentContainer = e.target.closest('.task-content');
                if (contentContainer) {
                    const index = contentContainer.getAttribute('data-index');
                    tasks[index].completed = !tasks[index].completed;
                    saveStateToStorage();
                    renderTasks();
                    return;
                }

                // Node deletion tracking processing
                if (e.target.classList.contains('btn-delete')) {
                    const index = e.target.getAttribute('data-index');
                    tasks.splice(index, 1);
                    saveStateToStorage();
                    renderTasks();
                }
            });

            // Action Triggers binding setup
            addTaskBtn.addEventListener('click', createNewTask);
            
            taskInput.addEventListener('keypress', (e) => {
                if (e.key === 'Enter') createNewTask();
            });

            // Launch execution layout rendering sequence on load
            renderTasks();
        });
    </script>
</body>
</html>
