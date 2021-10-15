<script lang="ts">
    interface Todo {
        id: number;
        task: string;
        done: boolean;
    }

    export let todoAddButtonText = 'Add Item test';

    let newTodoItem = '';

    let idCounter = JSON.parse(localStorage.getItem('idCounter')) ?? 0;
    let todoList: Todo[] = JSON.parse(localStorage.getItem('todoList')) ?? [];

    $: numberOfCompletedTasks = todoList
        .map(todoListItem => !!todoListItem.done ? 1 : 0)
        .reduce((previousValue, currentValue) => previousValue + currentValue, 0);

    $: {
        todoList = todoList.sort((a, b) => {
            if (a.done === b.done) return 0;
            else if (a.done) return 1;
            else return -1;
        })
        localStorage.setItem('todoList', JSON.stringify(todoList));
        localStorage.setItem('idCounter', JSON.stringify(idCounter));
    }

    function addTodoItem() {
        if (!newTodoItem) {
            return;
        }
        todoList = [ ...todoList, { id: idCounter++, task: newTodoItem, done: false } ];
        newTodoItem = '';
    }

    function deleteTodoItem(itemId: number) {
        todoList = todoList.filter(todoListItem => todoListItem.id !== itemId);
    }

    function clearAll() {
        todoList = [];
    }

</script>

<div class="todo-list__container">
    <h2 class="todo-list__title">Todo list (completed tasks = {numberOfCompletedTasks})</h2>

    <div class="todo-list__add-item-form">
        <input type="text" bind:value="{newTodoItem}"/>
        <button on:click={addTodoItem}>{todoAddButtonText}</button>
    </div>

    <div>
        <button on:click={clearAll}>Clear all</button>
    </div>

    {#if todoList.length > 0}
        <table class="todo-list__table">
            <colgroup>
                <col style="width: 90%"/>
                <col style="width: 10%"/>
            </colgroup>
            <thead>
            <tr class="todo-list__table-row">
                <th class="todo-list__table-heading">
                    Task
                </th>
                <th class="todo-list__table-heading">
                    Completed?
                </th>
                <th>
                    Actions
                </th>
            </tr>
            </thead>
            <tbody>
            {#each todoList as todoListItem}
                <tr class="todo-list__table-row" class:todo-list__table-row--done={todoListItem.done}>
                    <td>
                        {todoListItem.task}
                    </td>
                    <td>
                        <input type="checkbox" bind:checked={todoListItem.done}/>
                    </td>
                    <td>
                        <button on:click={deleteTodoItem(todoListItem.id)}>Delete</button>
                    </td>
                </tr>
            {/each}
            </tbody>
        </table>
    {:else}
        <p>You don't have any Todo's yet. Please add some!</p>
    {/if}
</div>

<style>
    .todo-list__container {
        display: grid;
        margin: 0 auto;
    }

    .todo-list__title {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 2em;
        font-weight: 100;
    }

    .todo-list__table {
        border: 2px solid #ff3e00;
        border-collapse: collapse;
        width: 50%;
    }

    .todo-list__table-heading {
    }

    th, td {
        border: 2px solid #ff3e00;
        padding: 6px;
    }

    td {
        text-align: left;
    }

    .todo-list__table-row {
        border: 2px solid #ff3e00;
    }

    .todo-list__table-row--done {
        border: 2px solid rgba(255, 62, 0, 0.6);
        opacity: 60%;
    }

    .todo-list__add-item-form {
        margin-top: 12px;
    }
</style>