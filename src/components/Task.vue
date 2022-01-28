<template>
    <div class="d-flex align-items-center justify-content-between">
        <input type="checkbox" v-model="task.completed" v-if="task.completed == false" />
        <span v-bind:class="{completed: task.completed == true}">{{ task.title }} - {{ task.completed }}</span>
        <button v-on:click="$emit('delete-task', task.id)" class="btn btn-danger">- Delete</button> 
        <button v-on:click="$emit('update-task', task.id)" class="btn btn-success"> Update</button> 
    </div>
</template>

<script>
export default {
    name: "Task",
    props: ["task"]
}
</script>

<style scoped>
.completed {text-decoration: line-through;}
</style>