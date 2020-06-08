<template>
    <div>
        <form @submit="addTodo">
            <input type="text" v-model="title" name="title" placeholder="Add Todo...">
            <input type="submit" value="Submit" class="btn">
        </form>
    </div>
</template>

<script>
// no longer used since the json placeholder has an id
//import { v4 as uuidv4 } from 'uuid';

export default {
    name: "Addtodo",
    data() {
        return {
            title: ''
        }
    },
    methods: {
        addTodo(e) {
            e.preventDefault();
            const newTodo = {
                //id: uuidv4(), no longer needed see above comment
                // just using numbers 1 or 2 for now
                userId: Math.ceil(Math.random()*2),
                title: this.title,
                completed: false
            }
            // Send up to parent
            this.$emit('add-todo', newTodo);

            this.title = '';
        }
    }
}
</script>

<style scoped>
    form {
        display: flex;
    }
    input[type="text"] {
        flex: 10;
        padding: 5px;
    }
    input[type="submit"] {
        flex: 2;
    }
</style>