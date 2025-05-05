---
layout: post
title: ToDo Tracker 
permalink: /todo
--- 

<link href="https://fonts.googleapis.com/css2?family=Patrick+Hand&display=swap" rel="stylesheet">

<style>
  .planner {
    background: repeating-linear-gradient(
      white,
      white 30px,
      #d1d1d1 31px,
      white 32px
    );
    border: 2px solid #ccc;
    border-radius: 12px;
    width: 500px;
    padding: 30px 40px;
    box-shadow: 4px 4px 10px rgba(0, 0, 0, 0.15);
    position: relative;
    font-family: 'Patrick Hand', cursive;
    margin: 2rem auto;
  }

  .planner::before {
    content: "";
    position: absolute;
    left: 40px;
    top: 0;
    bottom: 0;
    width: 2px;
    background-color: #ff6666;
  }

  .planner h1 {
    text-align: center;
    font-size: 32px;
    margin-bottom: 20px;
    color: #333;
  }

  .planner ul {
    list-style: none;
    padding: 0;
  }

  .planner li {
    font-size: 20px;
    padding: 6px 0;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .planner li.done span:nth-child(2) {
    text-decoration: line-through;
    color: gray;
  }

  .planner .task-controls {
    display: flex;
    gap: 10px;
    margin-top: 20px;
    flex-wrap: wrap;
  }

  .planner input[type="text"] {
    flex: 1;
    padding: 8px;
    font-size: 18px;
    border: 1px solid #aaa;
    border-radius: 6px;
    font-family: inherit;
  }

  .planner button {
    padding: 8px 12px;
    font-size: 16px;
    font-family: inherit;
    background-color: #ffe6e6;
    border: 1px solid #999;
    border-radius: 6px;
    cursor: pointer;
    transition: 0.2s;
  }

  .planner button:hover {
    background-color: #ffcccc;
  }
</style>

<div class="planner">
  <h1>My To-Do List</h1>
  <ul id="taskList"></ul>

  <div class="task-controls">
    <input type="text" id="taskInput" placeholder="New task...">
    <button onclick="addTask()">Add</button>
    <button onclick="saveTasks()">Save Tasks</button>
    <button onclick="loadTasks()">Load Previous Tasks</button>
    <button onclick="clearTasks()">Clear all Tasks</button> <!-- ✅ New button -->
  </div>
</div>

<script>
  let tasks = [];

  function renderTasks(tasks) {
    const taskList = document.getElementById("taskList");
    taskList.innerHTML = "";

    tasks.forEach((task, index) => {
      const li = document.createElement("li");
      if (task.done) li.classList.add("done");

      li.innerHTML = `
        <span onclick="markDone(${index})" style="cursor: pointer; font-size: 22px;">
          ${task.done ? "☑" : "☐"}
        </span>
        <span>${task.text}</span>
      `;

      taskList.appendChild(li);
    });
  }

  function addTask() {
    const input = document.getElementById("taskInput");
    if (input.value.trim()) {
      tasks.push({ text: input.value.trim(), done: false });
      input.value = "";
      renderTasks(tasks);
    }
  }

  function markDone(index) {
    tasks[index].done = !tasks[index].done;
    renderTasks(tasks);
  }

  function saveTasks() {
    localStorage.setItem("plannerTasks", JSON.stringify(tasks));
    alert("Tasks saved!");
  }

  function loadTasks() {
    const saved = localStorage.getItem("plannerTasks");
    if (saved) {
      tasks = JSON.parse(saved);
      renderTasks(tasks);
      alert("Tasks loaded!");
    } else {
      alert("No saved tasks found.");
    }
  }

  function clearTasks() {
    if (confirm("Are you sure you want to clear all tasks?")) {
      tasks = [];
      localStorage.removeItem("plannerTasks");
      renderTasks(tasks);
    }
  }
</script>
