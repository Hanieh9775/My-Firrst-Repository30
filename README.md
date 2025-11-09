<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Neon To-Do List</title>
<style>
    body {
        background: linear-gradient(135deg, #0a0a0a, #1c1c1c);
        color: #fff;
        font-family: "Poppins", sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
    }
    .todo-container {
        background: #111;
        padding: 30px;
        border-radius: 20px;
        box-shadow: 0 0 25px #00eaff;
        width: 400px;
        text-align: center;
    }
    h2 {
        color: #00eaff;
        margin-bottom: 20px;
    }
    input {
        width: 80%;
        padding: 10px;
        border: none;
        border-radius: 8px;
        font-size: 16px;
        outline: none;
        margin-bottom: 15px;
    }
    button {
        padding: 10px 15px;
        border: none;
        border-radius: 8px;
        background: #00eaff;
        color: #000;
        font-weight: bold;
        cursor: pointer;
        transition: 0.3s;
    }
    button:hover {
        background: #0098b0;
    }
    ul {
        list-style: none;
        padding: 0;
        margin: 0;
        text-align: left;
        margin-top: 15px;
    }
    li {
        background: #1f1f1f;
        padding: 10px;
        border-radius: 8px;
        margin-bottom: 10px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        animation: fadeIn 0.3s ease-in-out;
    }
    .delete {
        background: #ff4d4d;
        border: none;
        color: #fff;
        padding: 6px 10px;
        border-radius: 6px;
        cursor: pointer;
        transition: 0.3s;
    }
    .delete:hover {
        background: #cc0000;
    }
    .done {
        text-decoration: line-through;
        color: #888;
    }
    @keyframes fadeIn {
        from {opacity: 0; transform: translateY(5px);}
        to {opacity: 1; transform: translateY(0);}
    }
</style>
</head>
<body>

<div class="todo-container">
    <h2>✅ Neon To-Do List</h2>
    <input type="text" id="taskInput" placeholder="Enter a new task...">
    <button onclick="addTask()">Add</button>
    <ul id="taskList"></ul>
</div>

<script>
const taskInput = document.getElementById("taskInput");
const taskList = document.getElementById("taskList");

window.onload = loadTasks;

function addTask() {
    const taskText = taskInput.value.trim();
    if (taskText === "") return;

    const li = document.createElement("li");
    li.innerHTML = `<span onclick="toggleDone(this)">${taskText}</span>
                    <button class="delete" onclick="removeTask(this)">X</button>`;
    taskList.appendChild(li);
    taskInput.value = "";
    saveTasks();
}

function toggleDone(span) {
    span.classList.toggle("done");
    saveTasks();
}

function removeTask(button) {
    button.parentElement.remove();
    saveTasks();
}

function saveTasks() {
    const tasks = [];
    document.querySelectorAll("#taskList li").forEach(li => {
        tasks.push({
            text: li.querySelector("span").innerText,
            done: li.querySelector("span").classList.contains("done")
        });
    });
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function loadTasks() {
    const stored = JSON.parse(localStorage.getItem("tasks")) || [];
    stored.forEach(t => {
        const li = document.createElement("li");
        li.innerHTML = `<span onclick="toggleDone(this)" class="${t.done ? 'done' : ''}">${t.text}</span>
                        <button class="delete" onclick="removeTask(this)">X</button>`;
        taskList.appendChild(li);
    });
}
</script>

</body>
</html>
