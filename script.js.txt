const taskInput = document.getElementById('taskInput');
const addBtn = document.getElementById('addBtn');
const taskList = document.getElementById('taskList');

let tasks = JSON.parse(localStorage.getItem('tasks')) || [];

function saveTasks() {
localStorage.setItem('tasks', JSON.stringify(tasks));
}

function renderTasks() {
taskList.innerHTML = '';
tasks.forEach((task, index) => {
const li = document.createElement('li');
li.className = task.completed? 'completed' : '';
li.innerHTML = `
<span onclick="toggleTask(${index})">${task.text}</span>
<button class="deleteBtn" onclick="deleteTask(${index})">X</button>
`;
taskList.appendChild(li);
});
}

function addTask() {
const text = taskInput.value.trim();
if (text === '') return;
tasks.push({ text, completed: false });
taskInput.value = '';
saveTasks();
renderTasks();
}

function deleteTask(index) {
tasks.splice(index, 1);
saveTasks();
renderTasks();
}

function toggleTask(index) {
tasks[index].completed =!tasks[index].completed;
saveTasks();
renderTasks();
}

addBtn.addEventListener('click', addTask);
taskInput.addEventListener('keypress', (e) => {
if (e.key === 'Enter') addTask();
});

renderTasks();