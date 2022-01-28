<template>
    <div>
        <input type="text" class="form-control my-2" placeholder="Enter Task Here" v-model="title" v-on:keyup.enter="addNewTask" />
    </div>
</template>

<script>
import { v4 } from 'uuid';

export default {
    name: "AddTask",

    data() {
        return {
            title: '',

        }
    },

    methods: {
        addNewTask(){
            const task = {
                id: v4(),
                title: this.title,
                completed: false,
            };

            this.$emit('add-new-task', task);

            this.title = '';
        }
    }
}
</script>

<style scoped>

</style>