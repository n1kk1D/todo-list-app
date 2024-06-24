<script lang="ts">
    //Define the todo type
    type Todo = {
        id: number;
        name: string;
        isChecked: boolean;
    };

    //initialize the todos array
    let todos: Todo[] = [];
    //string for new todo input text 
    let newTodoText: string = '';

    //fetch todoes from the backend
    async function getTodos() {
        const todosData = await fetch("https://localhost:5000");

        const todos = await todosData.json(); 
        return todos;
    }

    // function onGetTodoButtonClick() {
    //     getTodos();
    // }
    
    //add new todo
    function createTodos() {
        if (newTodoText.trim() != '') {
            const newTodo: Todo = {
                id: Date.now(), //date is just basic storage
                name: newTodoText,
                isChecked: false
            };
            todos = [...todos, newTodo];
            newTodoText = '';
        }
    }


    function deleteTodo() {
        function deleteTodo(id: number) {
            todos = todos.filter(todo => todos.id !== id);
        }
    }

    function markTodoAsDone(id: number) {
        todos = todos.map(todo =>
            todo.id === id ? {...todo, done: !todo.done } : todo
        );
    }
</script>

{#each todos as todo}
    <p>- {todo}</p>
{/each}

<h1>My To-do list</h1>
<!-- Input for new todo -->
<div> 
    <input type="text" bind:value={newTodoText} placeholder="New todo">
    <button on:click={createTodos}>Add Todo</button>
</div>

<!-- Display the todo list -->
 <!-- {each todos as todo(todo.id)}
 {
    <button on:click={() => deleteTodo(todo.id)}>Delete</button>button>
    <input type="checkbox" bind:checked={todo.isChecked}>
 } -->
<div>
<button on:click={createTodos}>add Todo</button>
</div>
