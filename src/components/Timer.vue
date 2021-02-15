<template>
    <div class="container-fluid">
        <div class="mx-auto row">
            <h1 class="mb-3 mt-3 center-text">{{ title }}</h1>
            </div>
        <br/>
        <div class="row align-items-center justify-content-evenly">
        <input class="col-6 p-2 mx-3"
            type="text" name="tDesc" id="tDesc"
            v-model.lazy.trim="taskDescription"
            placeholder="taskDescription"
        >

            <input
        class="col-1 p-2 mx-3"
            v-model.lazy.number="taskTimeLimit"
            type="number" 
            name="tLimit" 
            id="tLimit"
            tabindex="0"
        >
        
        <button
            class="col-1 p-2 mx-3 btn btn-primary"
            :disabled="this.taskTimeLimit.value == '0'"
            @click.prevent="addTask"
        > + </button>
        </div>
        
    
</div>
<div class="container-fluid">
    <div class="mx-auto row justify-content-evenly">
        <Counter
        v-for="task in timers"
        :key="task.id"
        :task="task"
        @remove="removeTask"
    ></Counter>
    </div>
</div>
</template>

<script>
import Counter from './Counter.vue'

let currentId = 0 
const makeTimer = (timeLimit) => ({id: currentId++, limit: timeLimit});

export default{
    components:{
        Counter
    },
    props: { 
        title:{
            required: true,
            type: String
        }
    },
    data() {
        return {
            taskTimeLimit: 0,
            taskDescription: "Add a description",
            timers: [
                makeTimer(10),
                makeTimer(20),
                makeTimer(5)
            ]
        }
    },
    methods:{
        removeTask(taskId){
            console.log(taskId)
            this.timers = this.timers.filter(t => t.id !== taskId)
            currentId--
        },
        addTask(){
            console.log(this.taskTimeLimit)
            this.timers.push(makeTimer(this.taskTimeLimit))
        }
    }
}
</script>

<style>
    .center-text{
        text-align: center;
    }
</style>