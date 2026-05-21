# College-Project-
Java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaSim Pro | AI Logic & Memory Suite</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500&family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap');
        
        :root {
            --bg-deep: #080a0f;
            --bg-surface: #11141d;
            --bg-accent: #1a1e2a;
            --primary: #3b82f6;
            --primary-glow: rgba(59, 130, 246, 0.4);
            --java-orange: #f89820;
            --border: rgba(255, 255, 255, 0.08);
            --success: #10b981;
        }

        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: var(--bg-deep);
            color: #d1d5db;
            overflow: hidden;
            height: 100vh;
        }

        /* Neon Glow UI */
        .glow-active:focus-within {
            border-color: var(--primary);
            box-shadow: 0 0 20px var(--primary-glow);
        }

        .ai-pulse-btn {
            background: linear-gradient(135deg, #1e3a8a 0%, #3b82f6 100%);
            animation: pulse-glow 3s infinite;
        }

        @keyframes pulse-glow {
            0% { box-shadow: 0 0 5px rgba(59, 130, 246, 0.3); }
            50% { box-shadow: 0 0 15px rgba(59, 130, 246, 0.6); }
            100% { box-shadow: 0 0 5px rgba(59, 130, 246, 0.3); }
        }

        /* Editor Mechanics */
        .editor-layout {
            display: grid;
            grid-template-columns: 50px 1fr;
            background: var(--bg-deep);
            height: 100%;
            overflow-y: auto;
        }

        .line-nums {
            font-family: 'JetBrains Mono', monospace;
            padding-top: 24px;
            text-align: right;
            padding-right: 15px;
            color: #4b5563;
            background: rgba(0,0,0,0.3);
            user-select: none;
            font-size: 13px;
            border-right: 1px solid var(--border);
        }

        .code-input {
            font-family: 'JetBrains Mono', monospace;
            padding: 24px;
            background: transparent;
            color: #e2e8f0;
            line-height: 1.6;
            resize: none;
            outline: none;
            font-size: 14px;
            white-space: pre;
        }

        /* Variable Watcher Cards */
        .var-card {
            background: rgba(59, 130, 246, 0.03);
            border: 1px solid var(--border);
            border-radius: 8px;
            padding: 10px;
            margin-bottom: 8px;
            transition: all 0.2s;
        }
        .var-card:hover {
            border-color: var(--primary);
            background: rgba(59, 130, 246, 0.08);
        }

        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-thumb { background: #334155; border-radius: 10px; }
    </style>
</head>
<body class="flex flex-col">

    <!-- Top Navigation -->
    <nav class="h-14 border-b border-[var(--border)] bg-[var(--bg-surface)] flex items-center justify-between px-6 z-50">
        <div class="flex items-center space-x-5">
            <div class="flex items-center space-x-2">
                <i class="fa-brands fa-java text-[var(--java-orange)] text-2xl"></i>
                <span class="font-bold text-white tracking-tight">JavaSim <span class="text-blue-500">PBL Final</span></span>
            </div>
            <div class="h-6 w-[1px] bg-slate-800"></div>
            <div class="flex items-center gap-4">
                <button onclick="toggleProblemModal()" class="text-xs font-bold text-gray-500 hover:text-white transition flex items-center gap-2">
                    <i class="fa-solid fa-list-check"></i> Problem Set
                </button>
                <button onclick="exportCode()" class="text-xs font-bold text-gray-500 hover:text-white transition flex items-center gap-2">
                    <i class="fa-solid fa-download"></i> Save Source
                </button>
            </div>
        </div>

        <div class="flex items-center space-x-4">
            <button> onclick="toggleHelp()" class="text-gray-500 hover:text-white transition">
                <i class="fa-solid fa-circle-question text-lg"></i>
            </button>
            <button onclick="askAITutor()" class="ai-pulse-btn text-white px-5 py-2 rounded-full text-xs font-bold flex items-center gap-2 shadow-lg">
                <i class="fa-solid fa-robot"></i> AI Mentor Hint
            </button>
            <button onclick="runCode()" class="bg-green-600 hover:bg-green-500 text-white px-8 py-2 rounded-lg text-xs font-bold uppercase transition flex items-center gap-2 shadow-xl shadow-green-900/20">
                <i class="fa-solid fa-play"></i> Run Code
            </button>
        </div>
    </nav>

    <div class="flex-1 flex overflow-hidden">
        
        <!-- Sidebar: Task Explorer -->
        <aside class="w-72 bg-[var(--bg-surface)] border-r border-[var(--border)] flex flex-col">
            <div class="p-4 border-b border-[var(--border)] flex justify-between items-center">
                <span class="text-[10px] font-bold text-gray-500 uppercase tracking-widest">Library Explorer</span>
                <i class="fa-solid fa-folder-open text-blue-500/50"></i>
            </div>
            <div id="task-list" class="flex-1 overflow-y-auto">
                <!-- Task items injected -->
            </div>
            <div class="p-4 bg-[var(--bg-accent)] border-t border-[var(--border)]">
                <div class="flex items-center justify-between text-[10px] font-bold text-gray-500 uppercase mb-2">
                    <span>Simulation Engine</span>
                    <span class="text-green-500">Active</span>
                </div>
                <div class="text-[11px] text-gray-500 italic">No errors detected in JVM pipe.</div>
            </div>
        </aside>

        <!-- Main Workspace -->
        <main class="flex-1 flex flex-col min-w-0 bg-[var(--bg-deep)]">
            <div class="h-9 border-b border-[var(--border)] flex items-center px-4 bg-[var(--bg-surface)]">
                <div class="text-blue-400 text-[11px] font-semibold flex items-center gap-2 px-3 py-1.5 bg-blue-500/5 border-x border-t border-[var(--border)] rounded-t-md">
                    <i class="fa-solid fa-code"></i> Main.java
                </div>
            </div>

            <div class="flex-1 editor-layout glow-active">
                <div id="line-nums" class="line-nums">1</div>
                <textarea id="code-editor" class="code-input" spellcheck="false" oninput="updateLineNumbers()"></textarea>
            </div>

            <!-- Terminal & Input -->
            <div class="h-64 border-t border-[var(--border)] flex bg-[var(--bg-surface)]">
                <div class="w-80 border-r border-[var(--border)] flex flex-col">
                    <div class="px-4 py-2 text-[10px] font-bold text-gray-500 border-b border-[var(--border)] bg-black/10">SCANNER INPUT (STDIN)</div>
                    <textarea id="user-input" class="flex-1 bg-black/20 p-4 text-xs font-mono text-blue-300 resize-none outline-none" placeholder="Enter scanner inputs here (one per line)..."></textarea>
                </div>
                <div class="flex-1 flex flex-col">
                    <div class="px-4 py-2 text-[10px] font-bold text-gray-500 border-b border-[var(--border)] bg-black/10 flex justify-between">
                        <span>CONSOLE OUTPUT</span>
                        <button onclick="clearTerminal()" class="text-blue-500 text-[9px] font-bold">CLEAR</button>
                    </div>
                    <div id="terminal" class="flex-1 p-4 overflow-auto text-xs font-mono leading-relaxed">
                        <div class="text-gray-600 italic">// Ready to execute.</div>
                    </div>
                </div>
            </div>
        </main>

        <!-- Variable Watcher (Memory Map) -->
        <aside class="w-72 bg-[var(--bg-surface)] border-l border-[var(--border)] flex flex-col">
            <div class="p-4 border-b border-[var(--border)] flex justify-between items-center bg-black/10">
                <span class="text-[10px] font-bold text-gray-500 uppercase tracking-widest">Memory Watcher</span>
                <i class="fa-solid fa-microchip text-blue-500/50"></i>
            </div>
            <div id="watcher-list" class="p-4 overflow-y-auto flex-1">
                <div class="text-[11px] text-gray-600 italic text-center mt-6">Run code to see variable state...</div>
            </div>
        </aside>
    </div>

    <!-- Modals -->
    <div id="modal-container" class="fixed inset-0 z-[100] hidden flex items-center justify-center p-4 bg-black/80 backdrop-blur-md">
        <!-- Content injected dynamically -->
    </div>

    <script>
        const apiKey = ""; // Canvas Runtime API Key
        const terminal = document.getElementById('terminal');
        const codeEditor = document.getElementById('code-editor');
        const userInput = document.getElementById('user-input');
        const lineNums = document.getElementById('line-nums');
        const watcherList = document.getElementById('watcher-list');
        const taskList = document.getElementById('task-list');
        const modal = document.getElementById('modal-container');

        const DEFAULT_CODE = `import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("--- Sum Explorer ---");
        
        System.out.print("Enter A: ");
        int a = sc.nextInt();
        
        System.out.print("Enter B: ");
        int b = sc.nextInt();
        
        int result = a + b;
        System.out.println("Resulting Sum: " + result);
    }
}`;

        let currentTasks = JSON.parse(localStorage.getItem('pbl_tasks')) || [
            { id: 1, title: "Arithmetic Logic", desc: "Take two inputs and print their sum. Must contain 'Resulting Sum:'.", verify: "Resulting Sum" },
            { id: 2, title: "Odd/Even Checker", desc: "Input a number and print 'Number is Even' or 'Number is Odd'.", verify: "Number is" }
        ];

        let activeTask = null;

        // --- Init & UI ---
        window.onload = () => {
            codeEditor.value = localStorage.getItem('pbl_code_final') || DEFAULT_CODE;
            updateLineNumbers();
            renderTasks();
        };

        function updateLineNumbers() {
            const lines = codeEditor.value.split('\n').length;
            lineNums.innerHTML = Array.from({length: lines}, (_, i) => i + 1).join('<br>');
            localStorage.setItem('pbl_code_final', codeEditor.value);
        }

        function renderTasks() {
            taskList.innerHTML = currentTasks.map(t => `
                <div onclick="selectTask(${t.id})" class="p-4 border-b border-[var(--border)] cursor-pointer hover:bg-white/5 transition ${activeTask?.id === t.id ? 'bg-blue-500/10 border-l-4 border-l-blue-500' : ''}">
                    <div class="text-xs font-bold text-gray-200">${t.title}</div>
                    <div class="text-[10px] text-gray-500 mt-1 line-clamp-1">${t.desc}</div>
                </div>
            `).join('');
        }

        function selectTask(id) {
            activeTask = currentTasks.find(t => t.id === id);
            renderTasks();
            appendOutput(`\n[SYSTEM]: Active Task Loaded - ${activeTask.title}`, 'text-blue-400 font-bold border-l-2 border-blue-500 pl-3 my-2');
        }

        function toggleHelp() {
            modal.innerHTML = `
                <div class="bg-[var(--bg-surface)] border border-[var(--border)] w-full max-w-lg rounded-xl p-8 shadow-2xl">
                    <h2 class="text-xl font-bold mb-4">System Documentation</h2>
                    <div class="space-y-4 text-sm text-gray-400">
                        <p>1. <b>Memory Watcher:</b> This unique feature visualizes the state of your variables after execution finishes.</p>
                        <p>2. <b>Scanner:</b> Provide inputs in the bottom-left panel. One value per line.</p>
                        <p>3. <b>AI Mentor:</b> Uses Gemini to analyze your logic and provide hints without giving the full code.</p>
                    </div>
                    <button onclick="closeModal()" class="mt-8 w-full bg-blue-600 py-2 rounded-lg font-bold">Close</button>
                </div>
            `;
            modal.classList.remove('hidden');
        }

        function toggleProblemModal() {
            modal.innerHTML = `
                <div class="bg-[var(--bg-surface)] border border-[var(--border)] w-full max-w-md rounded-xl p-6 shadow-2xl">
                    <h2 class="text-lg font-bold mb-4">Add Custom PBL Task</h2>
                    <div class="space-y-4">
                        <input id="t-title" type="text" placeholder="Task Title" class="w-full bg-[var(--bg-deep)] border border-[var(--border)] p-3 rounded-lg text-sm outline-none focus:border-blue-500">
                        <textarea id="t-desc" placeholder="Task Description" class="w-full bg-[var(--bg-deep)] border border-[var(--border)] p-3 rounded-lg text-sm h-24 outline-none focus:border-blue-500"></textarea>
                        <input id="t-verify" type="text" placeholder="Success Keyword" class="w-full bg-[var(--bg-deep)] border border-[var(--border)] p-3 rounded-lg text-sm outline-none focus:border-blue-500">
                    </div>
                    <div class="flex gap-3 mt-6">
                        <button onclick="closeModal()" class="flex-1 px-4 py-2 text-gray-400 text-sm">Cancel</button>
                        <button onclick="saveTask()" class="flex-1 bg-blue-600 py-2 rounded-lg font-bold text-sm">Create Task</button>
                    </div>
                </div>
            `;
            modal.classList.remove('hidden');
        }

        function saveTask() {
            const title = document.getElementById('t-title').value;
            const desc = document.getElementById('t-desc').value;
            const verify = document.getElementById('t-verify').value;
            if(!title) return;
            currentTasks.push({ id: Date.now(), title, desc, verify });
            localStorage.setItem('pbl_tasks', JSON.stringify(currentTasks));
            renderTasks();
            closeModal();
        }

        function closeModal() { modal.classList.add('hidden'); }

        function appendOutput(text, className = 'text-white') {
            const entry = document.createElement('div');
            entry.className = `mb-1 ${className}`;
            entry.textContent = text;
            terminal.appendChild(entry);
            terminal.scrollTop = terminal.scrollHeight;
        }

        function clearTerminal() { terminal.innerHTML = '<div class="text-gray-600 italic">// Terminal cleared.</div>'; }

        function exportCode() {
            const blob = new Blob([codeEditor.value], {type: 'text/plain'});
            const a = document.createElement('a');
            a.href = URL.createObjectURL(blob);
            a.download = 'Main.java';
            a.click();
        }

        // --- AI Mentor Logic ---
        async function askAITutor() {
            modal.innerHTML = `<div class="bg-[var(--bg-surface)] border border-blue-500/30 w-full max-w-lg rounded-xl p-8 shadow-2xl text-center"><div class="flex justify-center gap-2 mb-4 animate-bounce"><div class="w-2 h-2 bg-blue-500 rounded-full"></div><div class="w-2 h-2 bg-blue-500 rounded-full"></div><div class="w-2 h-2 bg-blue-500 rounded-full"></div></div><p class="text-gray-400 italic">Gemini is analyzing your program state...</p></div>`;
            modal.classList.remove('hidden');

            const sys = "You are a friendly Java Programming Mentor. Look at the student's code and problem description. Identify logical errors or gaps. Give a helpful HINT. Do not give the full code. Use Markdown.";
            const user = `TASK: ${activeTask ? activeTask.desc : 'No Task Selected'}\nCODE:\n${codeEditor.value}\nCONSOLE OUTPUT:\n${terminal.innerText}`;

            try {
                let retries = 0;
                let data;
                while(retries < 5) {
                    const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({ contents: [{ parts: [{ text: user }] }], systemInstruction: { parts: [{ text: sys }] } })
                    });
                    data = await response.json();
                    if(response.ok) break;
                    retries++;
                    await new Promise(r => setTimeout(r, Math.pow(2, retries) * 1000));
                }
                const result = data.candidates?.[0]?.content?.parts?.[0]?.text || "Mentor failed to connect. Check your internet.";
                modal.innerHTML = `
                    <div class="bg-[var(--bg-surface)] border border-blue-500/30 w-full max-w-xl rounded-xl p-8 shadow-2xl">
                        <div class="flex items-center gap-3 mb-6 text-blue-400 font-bold"><i class="fa-solid fa-robot"></i> Mentor's Insight</div>
                        <div class="text-sm text-gray-300 leading-relaxed max-h-[50vh] overflow-y-auto">${result.replace(/\n/g, '<br>')}</div>
                        <button onclick="closeModal()" class="mt-8 w-full bg-slate-800 py-2 rounded-lg text-xs font-bold uppercase">Back to Work</button>
                    </div>
                `;
            } catch (e) {
                modal.innerHTML = `<div class="bg-red-900/20 border border-red-500/30 p-8 rounded-xl">AI Connection Failed. Try again later.</div>`;
            }
        }

        // --- Execution Logic ---
        function runCode() {
            const raw = codeEditor.value;
            const inputData = userInput.value.trim().split(/\s+/);
            let ptr = 0;
            let outputBuffer = [];

            clearTerminal();
            appendOutput("Starting Java JVM Simulation...", "text-blue-500/50");

            setTimeout(() => {
                try {
                    const print = (m) => {
                        const s = String(m);
                        outputBuffer.push(s);
                        appendOutput(s, "text-white");
                    };

                    const sc = {
                        nextInt: () => parseInt(inputData[ptr++]) || 0,
                        nextDouble: () => parseFloat(inputData[ptr++]) || 0.0,
                        nextLine: () => inputData[ptr++] || "",
                        next: () => inputData[ptr++] || ""
                    };

                    // Robust Main Extractor
                    const startMatch = raw.match(/public\s+static\s+void\s+main/);
                    if (!startMatch) throw new Error("Main method signature not found.");

                    const startBrace = raw.indexOf('{', startMatch.index);
                    if (startBrace === -1) throw new Error("Missing opening brace '{' for main.");

                    // Character-by-character Brace Counting (The Fix)
                    let count = 1;
                    let endBrace = -1;
                    for (let i = startBrace + 1; i < raw.length; i++) {
                        if (raw[i] === '{') count++;
                        else if (raw[i] === '}') count--;
                        if (count === 0) { endBrace = i; break; }
                    }

                    if (endBrace === -1) throw new Error("Missing closing brace '}' for main.");

                    let body = raw.substring(startBrace + 1, endBrace).trim();

                    // Memory Map Extractor (Static Analysis)
                    const varRegex = /\b(int|double|String|boolean)\s+(\w+)\s*=\s*([^;]+)/g;
                    let detectedVars = [];
                    let match;
                    while ((match = varRegex.exec(body)) !== null) {
                        detectedVars.push({ type: match[1], name: match[2], value: match[3] });
                    }

                    // Transpile
                    body = body
                        .replace(/System\.out\.println\s*\(/g, "print(")
                        .replace(/System\.out\.print\s*\(/g, "print(")
                        .replace(/Scanner\s+\w+\s*=\s*new\s*Scanner\s*\(.*?\);/g, "")
                        .replace(/\w+\.nextInt\(\)/g, "sc.nextInt()")
                        .replace(/\w+\.nextLine\(\)/g, "sc.nextLine()")
                        .replace(/\w+\.next\(\)/g, "sc.next()")
                        .replace(/\b(int|double|String|boolean|float|long|char)\s+(?=\w+)/g, "let ");

                    const execute = new Function("print", "sc", body);
                    execute(print, sc);

                    // Update Memory Watcher UI
                    watcherList.innerHTML = detectedVars.map(v => `
                        <div class="var-card">
                            <div class="flex justify-between items-center mb-1">
                                <span class="text-[10px] text-gray-500 uppercase font-bold">${v.type}</span>
                                <span class="text-[11px] text-blue-400 font-mono">${v.name}</span>
                            </div>
                            <div class="text-xs text-white truncate px-2 py-1 bg-black/30 rounded border border-white/5">
                                ${v.value}
                            </div>
                        </div>
                    `).join('') || '<div class="text-[11px] text-gray-600 italic text-center mt-6">No variables in main scope.</div>';

                    // Verify Task
                    if (activeTask && activeTask.verify) {
                        const outText = outputBuffer.join(" ").toLowerCase();
                        if (outText.includes(activeTask.verify.toLowerCase())) {
                            appendOutput("\n[SUCCESS]: Task validation passed!", "text-green-500 font-bold");
                        } else {
                            appendOutput(`\n[HINT]: Simulation finished, but output didn't contain "${activeTask.verify}"`, "text-orange-400");
                        }
                    }

                } catch (e) {
                    appendOutput(`RUNTIME EXCEPTION: ${e.message}`, "text-red-500 font-bold");
                }
            }, 300);
        }

        // Tab Management
        codeEditor.onkeydown = function(e) {
            if (e.key === 'Tab') {
                e.preventDefault();
                const s = this.selectionStart;
                this.value = this.value.substring(0, s) + "    " + this.value.substring(this.selectionEnd);
                this.selectionEnd = s + 4;
                updateLineNumbers();
            }
        };
    </script>
</body>
</html>
