<script lang="ts">
    import { onMount } from "svelte";
    
    //Define the todo type
    type Todo = {
        id: number;
        description: string;
        isDone: boolean;
    };

    //initialize the todos array
    let todos: Todo[] = [];
    //string for new todo input text 
    let newTodoText: string = '';

    //fetch todos from the backend
    async function getTodos() {
        const response = await fetch("http://localhost:3000/todo-item");
        if (response.ok) {
            const todosData: Todo[] = await response.json(); //// Parse the JSON response into JavaScript objects
            todos = todosData;
        } else{
            console.error('Failed to fetch todos');
        }
    }

    //add get todo function for backend
    //.json on the response item
    
    //add new todo
    async function createTodos() {
        const newTodo = {
            //creates a new todo object with unique id, if todos array is not empty, new id one greater than last item's id, if todos array is empty = 1
            id: todos.length > 0 ? todos[todos.length - 1].id + 1 : 1, 
            description: newTodoText,
            isDone: false
        };
        const response = await fetch("http://localhost:3000/todo-item", {
        method: "POST", //This is a POST request, not the default GET
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({ description: newTodoText }), // The JSON we are sending
    });
    //Once the new todo is added, re-ask server for to-do items (NOTE: This is flawed in a number of ways. Why?)
    if (response.ok) {  
        await getTodos();
        newTodoText = '';
    } else {
        console.error('Failed to create todo');
        }
    }

    //delete a todo
    async function deleteTodo(id: number) {
        const response = await fetch(`http://localhost:3000/todo-item/${id}`, {
            method: "DELETE",
        });
        if (response.ok){
        todos = todos.filter(todo => todo.id !== id);//filters todos array to exclude array based on its id
        } else {
            console.error('Failed to delete todo');
        }
    }

    async function checkTodo(todo: TodoItem) {
        const path = todo.isDone ? 'check' : 'uncheck';
        const response = await fetch(`http://localhost:3000/todo-item/${todo.id}/${path}`, {
            method: "POST"
        });
        if (!response.ok) {
            console.error('Failed to update todo');
        }
    }

    onMount(async () => {
        await getTodos();
    });
</script>


<h1>To-do list</h1>
<!-- Input for new todo -->
<div> 
    <input type="text" bind:value={newTodoText} placeholder="Description">
    <button on:click={createTodos}>Add</button>
</div>

<!-- Display the todo list -->
 {#each todos as todo(todo.id)}
    <div>
        <input type="checkbox" bind:checked={todo.isDone} on:change={() => checkTodo(todo)}>
        <span class:checked={todo.isDone}>{todo.description}</span>
        <button on:click={() => deleteTodo(todo.id)}>Delete</button>
    </div>
 {/each}

 <style>
    .checked {
        text-decoration: line-through;
    }
</style>