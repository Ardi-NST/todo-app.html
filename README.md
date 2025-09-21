<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Todo List Modern</title>
    <style>
        :root {
            --bg: #f5f5f5;
            --bgCard: #fff;
            --text: #333;
            --main: #2c3e50;
            --mainLight: #ecf0f1;
            --danger: #e74c3c;
            --dangerHover: #c0392b;
            --success: #229954;
            --toast: #222d3d;
        }
        body.dark {
            --bg: #181c20;
            --bgCard: #23272e;
            --text: #eaeaea;
            --main: #8ab4f8;
            --mainLight: #31373e;
            --danger: #ff7070;
            --dangerHover: #d64b4b;
            --success: #4fd675;
            --toast: #eaeaea;
        }
        body {
            background-color: var(--bg);
            padding: 20px;
            color: var(--text);
            transition: background 0.3s, color 0.3s;
        }
        .container {
            max-width: 700px;
            margin: 0 auto;
            background-color: var(--bgCard);
            padding: 22px;
            border-radius: 12px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.09);
            transition: background 0.3s;
        }
        h1 {
            text-align: center;
            margin-bottom: 20px;
            color: var(--main);
            font-size: 2.1rem;
            letter-spacing: 1.5px;
        }
        .top-bar {
            display: flex;
            gap: 10px;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 15px;
        }
        #delete-all-btn, #complete-all-btn, #delete-done-btn {
            background: var(--danger);
            color: #fff;
            border: none;
            border-radius: 5px;
            padding: 6px 15px;
            cursor: pointer;
            font-size: 14px;
            margin-right: 5px;
            display: flex;
            align-items: center;
            gap: 4px;
        }
        #delete-all-btn:hover, #delete-done-btn:hover {
            background: var(--dangerHover);
        }
        #complete-all-btn {
            background: var(--success);
        }
        #complete-all-btn:hover {
            filter: brightness(0.85);
        }
        #toggle-dark-btn {
            background: var(--main);
            color: #fff;
            border: none;
            border-radius: 4px;
            padding: 5px 15px;
            cursor: pointer;
            font-size: 13px;
        }
        .input-section {
            display: flex;
            margin-bottom: 18px;
            gap: 8px;
            flex-wrap: wrap;
        }
        #task-input {
            flex: 2;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px 0 0 4px;
            font-size: 16px;
            background: var(--bg);
            color: var(--text);
            min-width: 120px;
        }    #priority-input,    #deadline input {
            padding: 10px 8px;
            border: 1px solid #ddd;
            background: var(--bg);
            color: var(--text);
            font-size: 16px;
        }
        #deadline-input {
            min-width: 120px;
        }
        #add-btn {
            padding: 10px 18px;
            background-color: var(--main);
            color: white;
            border: none;
            border-radius: 0 4px 4px 0;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        #add-btn:hover {
            background-color: #1a252f;
        }
        #search-input {
            border: 1px solid #ddd;
            border-radius: 3px;
            padding: 8px 10px;
            font-size: 15px;
            flex: 2;
            background: var(--bg);
            color: var(--text);
        }
        .filter-section, .sort-section {
            margin-bottom: 16px;
            display: flex;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap;
        }
        .filter-btn, .sort-btn {
            padding: 7px 15px;
            background-color: var(--mainLight);
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
            color: var(--text);
        }
        .filter-btn.active, .sort-btn.active {
            background-color: var(--main);
            color: #fff;
        }
        .stats {
            margin-bottom: 10px;
            font-size: 15px;
            color: var(--main);
        }
        #progress-bar-container {
            width: 100%;
            background: #ececec;
            border-radius: 10px;
            margin-bottom: 12px;
            height: 16px;
            overflow: hidden;
        }
        #progress-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--main), var(--success));
            width: 0%;
            transition: width 0.4s;
        }
        #task-list {
            list-style-type: none;
            min-height: 40px;
        }
        .task-item {
            display: flex;
            align-items: center;
            padding: 12px;
            border-bottom: 1px solid #eee;
            animation: fadeIn 0.5s;
            gap: 8px;
            background: transparent;
            transition: background 0.2s;
            cursor: grab;
            user-select: none;
        }
        .task-item.dragging {
            opacity: 0.4;
            background: #e0e0e0;
        }
        .task-item.drag-over {
            border-top: 3px solid var(--main);
        }
        .task-item:last-child {
            border-bottom: none;
        }
        .task-text {
            flex: 1;
            margin-left: 10px;
            font-size: 16px;
        }
        .completed .task-text {
            text-decoration: line-through;
            color: #7f8c8d;
        }
        .priority {
            font-size: 13px;
            font-weight: bold;
            padding: 2px 10px;
            border-radius: 30px;
            margin-left: 5px;
            margin-right: 5px;
            min-width: 66px;
            text-align: center;
        }
        .priority-rendah {
            background: #d4efdf;
            color: #229954;
        }
        .priority-sedang {
            background: #fcf3cf;
            color: #b7950b;
        }
        .priority-tinggi {
            background: #fadbd8;
            color: #c0392b;
        }
        .danger-deadline {
            border-left: 5px solid var(--danger);
            background: #fff5f5;
        }
        .deadline {
            font-size: 13px;
            margin: 0 8px;
            padding: 2px 8px;
            border-radius: 8px;
            background: #eee;
            color: #7f8c8d;
        }
        .deadline-overdue {
            color: var(--danger);
            background: #fae3e3;
            font-weight: bold;
        }
        .deadline-soon {
            color: #b7950b;
            background: #fcf3cf;
            font-weight: bold;
        }
        .delete-btn, .edit-btn, .save-btn, .cancel-btn {
            margin-left: 5px;
            background-color: var(--danger);
            color: white;
            border: none;
            border-radius: 4px;
            padding: 6px 12px;
            cursor: pointer;
            transition: background-color 0.3s;
            display: flex;
            align-items: center;
            gap: 4px;
        }
        .edit-btn, .save-btn, .cancel-btn {
            background-color: var(--main);
        }
        .edit-btn:hover, .save-btn:hover, .cancel-btn:hover {
            background-color: #274777;
        }
        .delete-btn:hover {
            background-color: var(--dangerHover);
        }
        .empty-state {
            text-align: center;
            padding: 20px;
            color: #7f8c8d;
        }
        /* Toast */
        .toast {
            position: fixed;
            bottom: 30px;
            right: 30px;
            background: var(--toast);
            color: #fff;
            padding: 12px 26px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.11);
            opacity: 0;
            z-index: 999;
            font-size: 1rem;
            pointer-events: none;
            transition: opacity 0.4s, bottom 0.4s;
        }
        .toast.show {
            opacity: 1;
            pointer-events: auto;
            bottom: 50px;
        }
        /* Mobile */
        @media (max-width: 600px) {
            .container { padding: 11px; }
            .input-section, .filter-section, .sort-section { flex-direction: column; align-items: stretch; gap: 7px; }
            #add-btn { border-radius: 0 0 4px 4px; }
            #task-input { border-radius: 4px 4px 0 0; }
            #progress-bar-container { height: 12px; }
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(12px);}
            to { opacity: 1; transform: none;}
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Todo List Modern</h1>
        <div class="top-bar">
            <div>
                <button id="delete-all-btn" title="Hapus semua">
                    <svg width="16" height="16" viewBox="0 0 20 20"><path fill="currentColor" d="M5 6v10c0 1.1.9 2 2 2h6c1.1 0 2-.9 2-2V6H5zm2 10V6h6v10H7zM9 8v6h2V8H9zM4 4V2h12v2H4z"/></svg>
                    Hapus Semua
                </button>
                <button id="delete-done-btn" title="Hapus selesai">
                    <svg width="16" height="16" viewBox="0 0 20 20"><path fill="currentColor" d="M5 6v10c0 1.1.9 2 2 2h6c1.1 0 2-.9 2-2V6H5zm2 10V6h6v10H7zM9 8v6h2V8H9zM4 4V2h12v2H4z"/></svg>
                    Hapus Selesai
                </button>
                <button id="complete-all-btn" title="Selesaikan semua">
                    <svg width="16" height="16" viewBox="0 0 20 20"><path fill="currentColor" d="M7.629 15.657l-4.243-4.243 1.414-1.414 2.829 2.828 7.071-7.071 1.414 1.414z"/></svg>
                    Selesaikan Semua
                </button>
            </div>
            <button id="toggle-dark-btn" title="Dark/Light mode">🌙</button>
        </div>
        <div id="progress-bar-container"><div id="progress-bar"></div></div>
        <div class="stats" id="stats"></div>
        <div class="input-section">
            <input type="text" id="task-input" placeholder="Tambahkan tugas baru...">
            <select id="priority-input">
                <option value="sedang">Prioritas Sedang</option>
                <option value="rendah">Prioritas Rendah</option>
                <option value="tinggi">Prioritas Tinggi</option>
            </select>
            <input type="date" id="deadline-input">
            <button id="add-btn" title="Tambah tugas">
                <svg width="17" height="17" viewBox="0 0 20 20"><path fill="currentColor" d="M10 2a1 1 0 0 1 1 1v6h6a1 1 0 1 1 0 2h-6v6a1 1 0 1 1-2 0v-6H3a1 1 0 1 1 0-2h6V3a1 1 0 0 1 1-1z"/></svg>
                Tambah
            </button>
        </div>
        <div class="input-section" style="margin-bottom:10px;">
            <input type="text" id="search-input" placeholder="Cari tugas...">
        </div>
        <div class="filter-section">
            <button class="filter-btn active" data-filter="all">Semua</button>
            <button class="filter-btn" data-filter="active">Aktif</button>
            <button class="filter-btn" data-filter="completed">Selesai</button>
        </div>
        <div class="sort-section">
            <span>Urutkan:</span>
            <button class="sort-btn active" data-sort="created">Waktu</button>
            <button class="sort-btn" data-sort="priority">Prioritas</button>
            <button class="sort-btn" data-sort="deadline">Deadline</button>
            <button class="sort-btn" data-sort="status">Status</button>
        </div>
        <ul id="task-list">
            <li class="empty-state">Tidak ada tugas. Tambahkan tugas baru!</li>
        </ul>
    </div>
    <div class="toast" id="toast"></div>
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // DOM
            const taskInput = document.getElementById('task-input');
            const addBtn = document.getElementById('add-btn');
            const priorityInput = document.getElementById('priority-input');
            const deadlineInput = document.getElementById('deadline-input');
            const taskList = document.getElementById('task-list');
            const filterBtns = document.querySelectorAll('.filter-btn');
            const deleteAllBtn = document.getElementById('delete-all-btn');
            const completeAllBtn = document.getElementById('complete-all-btn');
            const deleteDoneBtn = document.getElementById('delete-done-btn');
            const searchInput = document.getElementById('search-input');
            const stats = document.getElementById('stats');
            const sortBtns = document.querySelectorAll('.sort-btn');
            const toggleDarkBtn = document.getElementById('toggle-dark-btn');
            const progressBar = document.getElementById('progress-bar');
            const toast = document.getElementById('toast');
            // State
            let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
            let currentFilter = 'all';
            let editingTaskId = null;
            let searchTerm = '';
            let currentSort = 'created';
            let dragSrcIndex = null;
            let unsavedEdit = false;
            // Dark mode
            function setDarkMode(on) {
                document.body.classList.toggle('dark', on);
                toggleDarkBtn.textContent = on ? "☀️" : "🌙";
                localStorage.setItem('darkMode', on ? '1' : '0');
            }
            setDarkMode(localStorage.getItem('darkMode') === '1');
            toggleDarkBtn.addEventListener('click', () => setDarkMode(!document.body.classList.contains('dark')));
           // Toast
            function showToast(msg) {
                toast.textContent = msg;
                toast.classList.add('show');
                setTimeout(() => toast.classList.remove('show'), 2000);
            }
         // Stats & Progress
            function updateStats() {
                const total = tasks.length;
                const active = tasks.filter(t => !t.completed).length;
                const done = tasks.filter(t => t.completed).length;
                stats.textContent = `Total: ${total} | Aktif: ${active} | Selesai: ${done}`;
                progressBar.style.width = total === 0 ? "0%" : (done / total * 100) + "%";
            }
             // Render
            function renderTasks() {
                taskList.innerHTML = '';
                let filteredTasks = tasks
                    .filter(task => {
                        if (currentFilter === 'active') return !task.completed;
                        if (currentFilter === 'completed') return task.completed;
                        return true;
                    })
                    .filter(task => task.text.toLowerCase().includes(searchTerm.toLowerCase()));
        // Sorting
                if(currentSort === 'priority') {
                    const prioOrder = {tinggi: 1, sedang: 2, rendah: 3};
                    filteredTasks = filteredTasks.sort((a, b) => prioOrder[a.priority] - prioOrder[b.priority]);
                } else if(currentSort === 'deadline') {
                    filteredTasks = filteredTasks.sort((a, b) => {
                        if(!a.deadline) return 1;
                        if(!b.deadline) return -1;
                        return a.deadline.localeCompare(b.deadline);
                    });
                } else if(currentSort === 'status') {
                    filteredTasks = filteredTasks.sort((a, b) => a.completed - b.completed);
                } else {
                    filteredTasks = filteredTasks.sort((a, b) => a.id - b.id); // default: created time
                }
           if (filteredTasks.length === 0) {
                    taskList.innerHTML = '<li class="empty-state">Tidak ada tugas.</li>';
                    updateStats();
                    return;
                }
                const nowDate = new Date().toISOString().split('T')[0];
                filteredTasks.forEach((task, idx) => {
                    const li = document.createElement('li');
                    let warnDeadline = '';
                    let deadlineLabel = '';
                    if(task.deadline){
                        let classDeadline = "deadline";
                        if(task.deadline < nowDate && !task.completed) {
                            classDeadline += " deadline-overdue";
                            warnDeadline = "danger-deadline";
                        } else if(task.deadline === nowDate && !task.completed) {
                            classDeadline += " deadline-soon";
                        }
                        deadlineLabel = `<span class="${classDeadline}">${task.deadline < nowDate && !task.completed ? 'Lewat: ' : ''}${task.deadline}</span>`;
                    }
         {
              li.className = `task-item ${task.completed ? 'completed' : ''} ${warnDeadline}`;
                    li.dataset.id = task.id;
                    li.setAttribute('draggable', editingTaskId === null); // disable drag saat edit
           {
                    if (editingTaskId === task.id) {
                        li.innerHTML = `
                            <input type="checkbox" ${task.completed ? 'checked' : ''} disabled>
                            <input type="text" class="edit-input" value="${task.text}" style="flex:1; margin-left:10px;">
                            <select class="edit-priority-input">
                                <option value="sedang" ${task.priority === 'sedang' ? 'selected' : ''}>Prioritas Sedang</option>
                                <option value="rendah" ${task.priority === 'rendah' ? 'selected' : ''}>Prioritas Rendah</option>
                                <option value="tinggi" ${task.priority === 'tinggi' ? 'selected' : ''}>Prioritas Tinggi</option>
                            </select>
                            <input type="date" class="edit-deadline-input" value="${task.deadline || ''}">
                            <button class="save-btn" title="Simpan">
                                <svg width="15" height="15" viewBox="0 0 20 20"><path fill="currentColor" d="M7.629 15.657l-4.243-4.243 1.414-1.414 2.829 2.828 7.071-7.071 1.414 1.414z"/></svg>
                            </button>
                            <button class="cancel-btn" title="Batal">
                                <svg width="15" height="15" viewBox="0 0 20 20"><path fill="currentColor" d="M10 8.586l4.95-4.95 1.414 1.414-4.95 4.95 4.95 4.95-1.414 1.414-4.95-4.95-4.95 4.95-1.414-1.414 4.95-4.95-4.95-4.95L5.05 3.636z"/></svg>
                            </button>
                            <button class="delete-btn" title="Hapus">
                                <svg width="15" height="15" viewBox="0 0 20 20"><path fill="currentColor" d="M5 6v10c0 1.1.9 2 2 2h6c1.1 0 2-.9 2-2V6H5zm2 10V6h6v10H7zM9 8v6h2V8H9zM4 4V2h12v2H4z"/></svg>
                            </button>
                        `;
                    } else {
                        li.innerHTML = `
                            <input type="checkbox" ${task.completed ? 'checked' : ''}>
                            <span class="task-text">${task.text}</span>
                            <span class="priority priority-${task.priority}">${capitalize(task.priority)}</span>
                            ${deadlineLabel}
                            <button class="edit-btn" title="Edit">
                                <svg width="14" height="14" viewBox="0 0 20 20"><path fill="currentColor" d="M14.85 2.85a2.5 2.5 0 0 1 3.54 3.54l-10 10a2 2 0 0 1-1.01.53l-4 1a1 1 0 0 1-1.22-1.22l1-4a2 2 0 0 1 .53-1.01l10-10zM16.07 5.93l-2-2L15.14 3a.5.5 0 0 1 .7.7l-1.07 1.07z"/></svg>
                            </button>
                            <button class="delete-btn" title="Hapus">
                                <svg width="15" height="15" viewBox="0 0 20 20"><path fill="currentColor" d="M5 6v10c0 1.1.9 2 2 2h6c1.1 0 2-.9 2-2V6H5zm2 10V6h6v10H7zM9 8v6h2V8H9zM4 4V2h12v2H4z"/></svg>
                            </button>
                        `;
                    }
                    // Drag & Drop event
                    li.addEventListener('dragstart', (e) => {
                        if (editingTaskId !== null) return;
                        dragSrcIndex = idx;
                        li.classList.add('dragging');
                    });
                    li.addEventListener('dragend', (e) => {
                        li.classList.remove('dragging');
                    });
                    li.addEventListener('dragover', (e) => {
                        e.preventDefault();
                        if (editingTaskId !== null) return;
                        li.classList.add('drag-over');
                    });
                    li.addEventListener('dragleave', (e) => {
                        li.classList.remove('drag-over');
                    });
                    li.addEventListener('drop', (e) => {
                        e.preventDefault();
                        if (editingTaskId !== null) return;
                        li.classList.remove('drag-over');
                        if (dragSrcIndex === null || dragSrcIndex === idx) return;
                        // reorder
                        const filteredIndices = tasks
                            .map((t, i) => ({t, i}))
                            .filter(({t}) => {
                                if (currentFilter === 'active') return !t.completed;
                                if (currentFilter === 'completed') return t.completed;
                                return true;
                            })
                            .filter(({t}) => t.text.toLowerCase().includes(searchTerm.toLowerCase()))
                            .map(({i}) => i);
                        const src = filteredIndices[dragSrcIndex];
                        const dest = filteredIndices[idx];
                        if (src === undefined || dest === undefined) return;
                        const item = tasks.splice(src, 1)[0];
                        tasks.splice(dest, 0, item);
                        saveTasks();
                        renderTasks();
                        showToast("Tugas diurutkan ulang.");
                    });
                    taskList.appendChild(li);
                });
                updateStats();
            }
          }
         function capitalize(str) {
                if (str === "rendah") return "Rendah";
                if (str === "sedang") return "Sedang";
                if (str === "tinggi") return "Tinggi";
                return str.charAt(0).toUpperCase() + str.slice(1);
            }
          {
      // Tambah task baru
            function addTask() {
                const text = taskInput.value.trim();
                const priority = priorityInput.value;
                const deadline = deadlineInput.value;
                if (text === '') return;
                const newTask = {
                    id: Date.now(),
                    text: text,
                    priority: priority,
                    completed: false,
                    deadline: deadline
                };
                tasks.push(newTask);
                saveTasks();
                renderTasks();
                showToast("Tugas ditambahkan!");
                taskInput.value = '';
                priorityInput.value = 'sedang';
                deadlineInput.value = '';
                taskInput.focus();
            }
       {
            // Toggle status task
            function toggleTask(id) {
                tasks = tasks.map(task => {
                    if (task.id === id) {
                        return { ...task, completed: !task.completed };
                    }
                    return task;
                });
                saveTasks();
                renderTasks();
            }
            {
            // Hapus task dengan konfirmasi
            function deleteTask(id) {
                if (!confirm('Yakin ingin menghapus tugas ini?')) return;
                tasks = tasks.filter(task => task.id !== id);
                saveTasks();
                renderTasks();
                showToast("Tugas dihapus.");
            }
              // Edit task
            function startEditTask(id) {
                if (editingTaskId && unsavedEdit) {
                    if (!confirm("Perubahan belum disimpan. Lanjut edit tugas lain?")) return;
                }
                editingTaskId = id;
                unsavedEdit = false;
                renderTasks();
                const input = document.querySelector('.edit-input');
                if (input) input.focus();
            }
            function saveEditTask(id, newText, newPriority, newDeadline) {
                tasks = tasks.map(task => {
                    if (task.id === id) {
                        return { ...task, text: newText, priority: newPriority, deadline: newDeadline };
                    }
                    return task;
                });
                editingTaskId = null;
                unsavedEdit = false;
                saveTasks();
                renderTasks();
                showToast("Tugas diedit.");
            }
            function cancelEditTask() {
                editingTaskId = null;
                unsavedEdit = false;
                renderTasks();
            }
   // Hapus semua task
            function deleteAllTasks() {
                if (tasks.length === 0) return;
                if (confirm('Yakin ingin menghapus SEMUA tugas?')) {
                    tasks = [];
                    saveTasks();
                    renderTasks();
                    showToast("Semua tugas dihapus.");
                }
            }
         // Hapus semua selesai
            function deleteDoneTasks() {
                if (tasks.filter(t=>t.completed).length === 0) return;
                if(confirm('Yakin ingin menghapus semua tugas yang sudah selesai?')) {
                    tasks = tasks.filter(t=>!t.completed);
                    saveTasks();
                    renderTasks();
                    showToast("Tugas selesai dihapus.");
                }
            }
// Tandai semua selesai
            function completeAllTasks() {
                if(tasks.filter(t=>!t.completed).length === 0) return;
                if(confirm('Tandai semua tugas sebagai selesai?')) {
                    tasks = tasks.map(t=>({...t, completed:true}));
                    saveTasks();
                    renderTasks();
                    showToast("Semua tugas selesai!");
                }
            }
  // Simpan tasks ke localStorage
            function saveTasks() {
                localStorage.setItem('tasks', JSON.stringify(tasks));
            }
  // Keyboard shortcut & autofokus
            document.addEventListener('keydown', function(e) {
                if ((e.ctrlKey || e.metaKey) && e.key === 'Enter' && document.activeElement === taskInput) {
                    addTask();
                }
            });
              // Event listeners
            addBtn.addEventListener('click', addTask);
            taskInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') addTask();
            });
            priorityInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') addTask();
            });
            deadlineInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') addTask();
            });
            deleteAllBtn.addEventListener('click', deleteAllTasks);
            deleteDoneBtn.addEventListener('click', deleteDoneTasks);
            completeAllBtn.addEventListener('click', completeAllTasks);
         taskList.addEventListener('click', function(e) {
                const taskItem = e.target.closest('.task-item');
                if (!taskItem) return;
                const taskId = parseInt(taskItem.dataset.id);
if (e.target.type === 'checkbox' && !e.target.disabled) {
                    toggleTask(taskId);
                }
                if (e.target.classList.contains('delete-btn')) {
                    deleteTask(taskId);
                }
                if (e.target.classList.contains('edit-btn')) {
                    startEditTask(taskId);
                }
                if (e.target.classList.contains('save-btn')) {
                    const newText = taskItem.querySelector('.edit-input').value.trim();
                    const newPriority = taskItem.querySelector('.edit-priority-input').value;
                    const newDeadline = taskItem.querySelector('.edit-deadline-input').value;
                    if (newText !== '') {
                        saveEditTask(taskId, newText, newPriority, newDeadline);
                    }
                }
                if (e.target.classList.contains('cancel-btn')) {
                    cancelEditTask();
                }
            });
            taskList.addEventListener('keypress', function(e) {
                if (e.target.classList.contains('edit-input')) {
                    unsavedEdit = true;
                    if (e.key === 'Enter') {
                        const taskItem = e.target.closest('.task-item');
                        const taskId = parseInt(taskItem.dataset.id);
                        const newText = e.target.value.trim();
                        const newPriority = taskItem.querySelector('.edit-priority-input').value;
                        const newDeadline = taskItem.querySelector('.edit-deadline-input').value;
                        if (newText !== '') {
                            saveEditTask(taskId, newText, newPriority, newDeadline);
                        }
                    }
                }
            });
filterBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    filterBtns.forEach(b => b.classList.remove('active'));
                    this.classList.add('active');
                    currentFilter = this.dataset.filter;
                    renderTasks();
                });
            });
sortBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    sortBtns.forEach(b => b.classList.remove('active'));
                    this.classList.add('active');
                    currentSort = this.dataset.sort;
                    renderTasks();
                });
            });
            searchInput.addEventListener('input', function() {
                searchTerm = this.value;
                renderTasks();
            });
// Peringatan keluar jika edit belum disimpan
            window.addEventListener('beforeunload', function(e){
                if (editingTaskId && unsavedEdit) {
                    e.preventDefault();
                    e.returnValue = '';
                }
            });
    body {
 font-family: sans-serif;
  background: #f5f5f5;
  margin: 0;
  padding: 0;
}
#todo-section, #sync-section, #auth-section, #label-section, #share-section, #undo-redo-section, #notif-section, #calendar-section {
  background: #fff;
  margin: 16px auto;
  padding: 16px;
  max-width: 500px;
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.1);
}
.todo {
  margin: 8px 0;
  border-bottom: 1px solid #eee;
  padding-bottom: 8px;
}
.todo.done {
  text-decoration: line-through;
  color: #888;
}
@media (max-width:600px) {
  body { padding: 0; }
  #todo-section, #sync-section, #auth-section, #label-section, #share-section, #undo-redo-section, #notif-section, #calendar-section {
    max-width: 100%;
    margin: 8px 0;
    border-radius: 0;
  }
  } 
  {
  "name": "Todo App Advanced",
  "short_name": "TodoApp",
  "start_url": "./",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#1976d2",
  "icons": [
    {
      "src": "icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
 // === 1. Sinkronisasi dan Backup Cloud (Firebase/Supabase/JSON) ===
let db, auth, storage; // Placeholder untuk instance Firebase/Supabase
let todos = [];
let labels = [];
let undoStack = [], redoStack = [];
let currentUser = null, sharedWith = [];
function syncToCloud() {
  alert('Fitur Sync ke Cloud: implementasikan Firebase/Supabase SDK di sini.');
}
function importJSON() {
  alert('Import JSON: implementasi input file dan parsing JSON.');
}
function exportJSON() {
  alert('Export JSON: implementasi blob JSON dan trigger download.');
}
// === 2. Sistem Multi-user & Autentikasi ===
function showLogin() { document.getElementById('loginForm').style.display = 'block'; }
function showRegister() { document.getElementById('registerForm').style.display = 'block'; }
function login() { alert('Login: implementasikan dengan Firebase Auth atau backend Anda.'); }
function googleLogin() { alert('Login Google: implementasikan OAuth Google/Firebase.'); }
function register() { alert('Register: implementasikan register user.'); }
function logout() { alert('Logout: implementasikan fungsi logout.'); }
// === 3. Notifikasi Pengingat Otomatis ===
function requestNotifPermission() {
  if ('Notification' in window) Notification.requestPermission();
}
function showReminderNotification(todo) {
  if (Notification.permission === "granted") {
    new Notification("Pengingat tugas:", { body: todo.text });
  }
}
// === 4. Subtask/Checklist ===
function renderSubtasks(subtasks, todoIdx) {
  return subtasks.map((sub, i) =>
    `<li>
      <input type="checkbox" ${sub.done ? "checked" : ""} onchange="toggleSubtask(${todoIdx},${i})">
      ${sub.text}
      <button onclick="removeSubtask(${todoIdx},${i})">Hapus</button>
    </li>`
  ).join('');
}
function addSubtask(todoIdx) {
  let subtaskText = prompt("Subtask:");
  if (subtaskText) {
    todos[todoIdx].subtasks.push({ text: subtaskText, done: false });
    saveState(); renderTodos();
  }
}
function toggleSubtask(todoIdx, subIdx) {
  todos[todoIdx].subtasks[subIdx].done = !todos[todoIdx].subtasks[subIdx].done;
  saveState(); renderTodos();
}
function removeSubtask(todoIdx, subIdx) {
  todos[todoIdx].subtasks.splice(subIdx, 1);
  saveState(); renderTodos();
}
// === 5. Attachment/Upload File ===
function handleAttachment(files, todoIdx) {
  alert('Upload file: implementasi upload ke cloud storage.');
}
// === 6. Label/Kategori Kustom ===
function addLabel() {
  let label = document.getElementById('newLabel').value.trim();
  if (label && !labels.includes(label)) {
    labels.push(label); renderLabels();
  }
}
function renderLabels() {
  document.getElementById('labelList').innerHTML = labels.map(l => `<span>${l}</span>`).join(' ');
  let labelSelect = document.getElementById('labelSelect');
  labelSelect.innerHTML = labels.map(l => `<option value="${l}">${l}</option>`).join('');
}
// === 7. Integrasi & Tampilan Kalender ===
function showCalendar() {
  alert('Tampilan kalender: Gunakan library seperti FullCalendar atau buat tampilan custom.');
}
// === 8. Kolaborasi & Berbagi ===
function shareTodoList() {
  let email = document.getElementById('shareEmail').value;
  if (email) { alert('Fitur berbagi: Implementasikan undangan berbagi via email/user.'); }
}
// === 9. Undo/Redo ===
function saveState() {
  undoStack.push(JSON.stringify({ todos, labels }));
  redoStack = [];
}
function undo() {
  if (undoStack.length > 1) {
    redoStack.push(undoStack.pop());
    let prev = JSON.parse(undoStack[undoStack.length - 1]);
    todos = prev.todos; labels = prev.labels; renderTodos(); renderLabels();
  }
}
function redo() {
  if (redoStack.length > 0) {
    let next = JSON.parse(redoStack.pop());
    todos = next.todos; labels = next.labels; renderTodos(); renderLabels();
    undoStack.push(JSON.stringify({ todos, labels }));
  }
}
// === 10. PWA & Tampilan Mobile Optimal ===
// === Todo CRUD & Render ===
function addTodo(event) {
  event.preventDefault();
  let input = document.getElementById('todoInput').value.trim();
  let label = document.getElementById('labelSelect').value;
  let reminder = document.getElementById('reminderInput').value;
  let attachment = document.getElementById('attachmentInput').files;
  if (input) {
    let todo = {
      text: input, label: label, done: false,
      reminder: reminder, attachments: [], subtasks: []
    };
    todos.push(todo); saveState(); renderTodos();
    if (reminder) scheduleReminder(todo);
    if (attachment.length) handleAttachment(attachment, todos.length - 1);
    document.getElementById('todoForm').reset();
  }
}
function renderTodos() {
  document.getElementById('todoList').innerHTML = todos.map((todo, idx) =>
    `<div class="todo ${todo.done ? 'done' : ''}">
      <input type="checkbox" ${todo.done ? 'checked' : ''} onchange="toggleTodo(${idx})">
      <strong>${todo.text}</strong> [${todo.label || '-'}]
      <button onclick="addSubtask(${idx})">+Subtask</button>
      <ul>${renderSubtasks(todo.subtasks || [], idx)}</ul>
      <span>Reminder: ${todo.reminder || '-'}</span>
      <span>Attachment: ${todo.attachments?.length || 0}</span>
      <button onclick="deleteTodo(${idx})">Hapus</button>
    </div>`
  ).join('');
}
function toggleTodo(idx) { todos[idx].done = !todos[idx].done; saveState(); renderTodos(); }
function deleteTodo(idx) { todos.splice(idx, 1); saveState(); renderTodos(); }
function scheduleReminder(todo) {
  setTimeout(() => showReminderNotification(todo), 5000);
}
window.onload = function() {
  renderLabels();
  renderTodos();
  saveState();
};      
 // Render awal & auto focus
            renderTasks();
        taskInput.focus();
        });
    </script>
</body>
</html>
