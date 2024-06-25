<script lang="ts">
    //Define the todo type
    type Todo = {
        id: number;
        text: string;
        isChecked: boolean;
    };

    //initialize the todos array
    let todos: Todo[] = [];
    //string for new todo input text 
    let newTodoText: string = '';

    //fetch todos from the backend
    async function getTodos() {
        const todosData = await fetch("https://localhost:5000");
        const todos = await todosData.json(); 
        return todos;
    }

    //add get todo function for backend
    
    //add new todo
    function createTodos() {
        const newTodo: Todo = {
            //creates a new todo object with unique id, if todos array is not empty, new id one greater than last item's id, if todos array is empty = 1
            id: todos.length > 0 ? todos[todos.length - 1].id + 1 : 1, 
            text: newTodoText,
            isChecked: false
        };
        todos = [...todos, newTodo];
        newTodoText = '';
    }

    function deleteTodo(id: number) {
        todos = todos.filter(todo => todo.id !== id);//filters todos array to exclue array based on its id
    }
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
        <input type="checkbox" bind:checked={todo.isChecked}>
        <span class:checked={todo.isChecked}>{todo.text}</span>
        <button on:click={() => deleteTodo(todo.id)}>Delete</button>
    </div>
 {/each}

 <style>
    .checked {
        text-decoration: line-through;
    }
</style>