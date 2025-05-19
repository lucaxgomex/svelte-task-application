<script lang="ts">
    let newTask = $state("");
    let { 
        addTask,
    }: {
        addTask: (newTask: string) => void;
    } = $props();

    function formSubmitted(e: SubmitEvent) {
        e.preventDefault();
        addTask(newTask);
        newTask = "";
    }
</script>

<form action="" onsubmit="{formSubmitted}">
    <label for="">
        <input type="text" name="newTask" bind:value={newTask}/>
    </label>
    <div class="button-container">
        <button>
            Add
        </button>

        <button onclick={() => location.reload()}>
            Clear
        </button>
    </div>
</form>

<style>
    form {
        margin-bottom: 1rem;
    }

    .button-container {
        display: flex;
        justify-content: space-between;
    }
</style>