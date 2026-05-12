# Ex03 To-Do List using JavaScript
## Date:12-05-2026

## AIM
To create a To-do Application with all features using JavaScript.

## ALGORITHM
### STEP 1
Build the HTML structure (index.html).

### STEP 2
Style the App (style.css).

### STEP 3
Plan the features the To-Do App should have.

### STEP 4
Create a To-do application using Javascript.

### STEP 5
Add functionalities.

### STEP 6
Test the App.

### STEP 7
Open the HTML file in a browser to check layout and functionality.

### STEP 8
Fix styling issues and refine content placement.

### STEP 9
Deploy the website.

### STEP 10
Upload to GitHub Pages for free hosting.

## PROGRAM
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Advanced Todo Application</title>

    <style>

        *{
            margin:0;
            padding:0;
            box-sizing:border-box;
            font-family:Arial, sans-serif;
        }

        body{
            background:linear-gradient(to right,#4facfe,#00f2fe);
            min-height:100vh;
            display:flex;
            justify-content:center;
            align-items:center;
            padding:20px;
        }

        .container{
            width:100%;
            max-width:700px;
            background:white;
            padding:30px;
            border-radius:15px;
            box-shadow:0 5px 15px rgba(0,0,0,0.3);
        }

        h1{
            text-align:center;
            margin-bottom:25px;
            color:#333;
        }

        /* Input Section */

        .todo-input{
            display:flex;
            gap:10px;
            margin-bottom:20px;
            flex-wrap:wrap;
        }

        .todo-input input,
        .todo-input select{
            padding:12px;
            border:1px solid #ccc;
            border-radius:8px;
            font-size:16px;
        }

        #taskInput{
            flex:1;
        }

        button{
            padding:12px 18px;
            border:none;
            border-radius:8px;
            cursor:pointer;
            font-size:15px;
            transition:0.3s;
        }

        .add-btn{
            background:#007bff;
            color:white;
        }

        .add-btn:hover{
            background:#0056b3;
        }

        /* Filters */

        .filters{
            display:flex;
            justify-content:center;
            gap:10px;
            margin-bottom:20px;
            flex-wrap:wrap;
        }

        .filters button{
            background:#eee;
        }

        .filters button:hover{
            background:#ccc;
        }

        /* Todo List */

        ul{
            list-style:none;
        }

        li{
            background:#f9f9f9;
            margin-bottom:15px;
            padding:15px;
            border-radius:10px;
            display:flex;
            justify-content:space-between;
            align-items:center;
            flex-wrap:wrap;
            gap:10px;
            border-left:6px solid #007bff;
        }

        .completed{
            text-decoration:line-through;
            opacity:0.6;
        }

        .task-details{
            flex:1;
        }

        .priority{
            font-size:14px;
            font-weight:bold;
        }

        .high{
            color:red;
        }

        .medium{
            color:orange;
        }

        .low{
            color:green;
        }

        .actions{
            display:flex;
            gap:8px;
            flex-wrap:wrap;
        }

        .complete-btn{
            background:green;
            color:white;
        }

        .edit-btn{
            background:orange;
            color:white;
        }

        .delete-btn{
            background:red;
            color:white;
        }

        .clear-btn{
            width:100%;
            margin-top:20px;
            background:black;
            color:white;
        }

        .stats{
            text-align:center;
            margin-top:15px;
            font-weight:bold;
            color:#333;
        }

        @media(max-width:600px){

            li{
                flex-direction:column;
                align-items:flex-start;
            }

            .actions{
                width:100%;
            }

            .actions button{
                flex:1;
            }
        }

    </style>
</head>

<body>

    <div class="container">

        <h1>Advanced Todo Application</h1>

        <!-- Input Section -->

        <div class="todo-input">

            <input type="text" id="taskInput" placeholder="Enter your task">

            <input type="date" id="dueDate">

            <select id="priority">
                <option value="Low">Low Priority</option>
                <option value="Medium">Medium Priority</option>
                <option value="High">High Priority</option>
            </select>

            <button class="add-btn" onclick="addTask()">Add Task</button>

        </div>

        <!-- Filter Buttons -->

        <div class="filters">

            <button onclick="filterTasks('all')">All</button>
            <button onclick="filterTasks('completed')">Completed</button>
            <button onclick="filterTasks('pending')">Pending</button>

        </div>

        <!-- Todo List -->

        <ul id="taskList"></ul>

        <!-- Statistics -->

        <div class="stats">
            Total Tasks: <span id="totalTasks">0</span> |
            Completed: <span id="completedTasks">0</span> |
            Pending: <span id="pendingTasks">0</span>
        </div>

        <!-- Clear All -->

        <button class="clear-btn" onclick="clearAllTasks()">Clear All Tasks</button>

    </div>

    <script>

        let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

        function saveTasks(){
            localStorage.setItem("tasks", JSON.stringify(tasks));
        }

        function addTask(){

            const taskInput = document.getElementById("taskInput");
            const dueDate = document.getElementById("dueDate");
            const priority = document.getElementById("priority");

            if(taskInput.value.trim() === ""){
                alert("Please enter a task");
                return;
            }

            const task = {
                id: Date.now(),
                text: taskInput.value,
                completed: false,
                dueDate: dueDate.value,
                priority: priority.value
            };

            tasks.push(task);

            saveTasks();

            taskInput.value = "";
            dueDate.value = "";

            displayTasks();
        }

        function displayTasks(filter = "all"){

            const taskList = document.getElementById("taskList");

            taskList.innerHTML = "";

            let filteredTasks = tasks;

            if(filter === "completed"){
                filteredTasks = tasks.filter(task => task.completed);
            }

            else if(filter === "pending"){
                filteredTasks = tasks.filter(task => !task.completed);
            }

            filteredTasks.forEach(task => {

                const li = document.createElement("li");

                li.innerHTML = `

                    <div class="task-details">

                        <h3 class="${task.completed ? 'completed' : ''}">
                            ${task.text}
                        </h3>

                        <p>Due Date: ${task.dueDate || "No Date"}</p>

                        <p class="priority ${task.priority.toLowerCase()}">
                            ${task.priority} Priority
                        </p>

                    </div>

                    <div class="actions">

                        <button class="complete-btn"
                        onclick="toggleComplete(${task.id})">
                        ${task.completed ? 'Undo' : 'Complete'}
                        </button>

                        <button class="edit-btn"
                        onclick="editTask(${task.id})">
                        Edit
                        </button>

                        <button class="delete-btn"
                        onclick="deleteTask(${task.id})">
                        Delete
                        </button>

                    </div>
                `;

                taskList.appendChild(li);

            });

            updateStats();
        }

        function toggleComplete(id){

            tasks = tasks.map(task => {

                if(task.id === id){
                    task.completed = !task.completed;
                }

                return task;
            });

            saveTasks();
            displayTasks();
        }

        function deleteTask(id){

            tasks = tasks.filter(task => task.id !== id);

            saveTasks();
            displayTasks();
        }

        function editTask(id){

            const task = tasks.find(task => task.id === id);

            const newTask = prompt("Edit Task:", task.text);

            if(newTask !== null && newTask.trim() !== ""){

                task.text = newTask;

                saveTasks();
                displayTasks();
            }
        }

        function filterTasks(filter){
            displayTasks(filter);
        }

        function clearAllTasks(){

            if(confirm("Are you sure you want to clear all tasks?")){

                tasks = [];

                saveTasks();
                displayTasks();
            }
        }

        function updateStats(){

            document.getElementById("totalTasks").innerText = tasks.length;

            const completed = tasks.filter(task => task.completed).length;

            document.getElementById("completedTasks").innerText = completed;

            document.getElementById("pendingTasks").innerText =
            tasks.length - completed;
        }

        displayTasks();

    </script>

</body>
</html>
```

## OUTPUT

<img width="1912" height="915" alt="image" src="https://github.com/user-attachments/assets/877f4888-c941-4f0e-a699-ce13c7a81a7c" />


## RESULT
The program for creating To-do list using JavaScript is executed successfully.
