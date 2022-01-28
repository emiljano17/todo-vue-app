<template>
    <div>
        <div v-for="t in tasks" :key="t.id">
            <Task :task="t" v-on:delete-task="$emit('delete-task', t.id)" v-on:update-task="$emit('update-task', t.id)" />
        </div>
    </div>
</template>

<script>
import Task from "./Task.vue";

export default {
    name: "Tasks",
    components: {
        Task
    },
    props: ["tasks"],
}
</script>

<style scoped>

</style>