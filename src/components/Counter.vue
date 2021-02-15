<template>
        <div class="center-text col-3 col-xs-12 col-sm-12 col-md-3 p-3">
        <p>Time elapsed ... {{ counter }} </p>
        <p>Time remaining ... {{ timeRemaining }}</p>
        <button class="mx-1 btn btn-secondary" @click="startTimer" tabindex="1"> Start! </button>
        <button class="mx-1 btn btn-secondary" @click="stopTimer" type="reset" tabindex="2">Reset! </button>
        <button class="mx-1 btn btn-danger" @click.prevent="$emit('remove', tid)"> - </button>
        </div>
</template>

<script>
import { ref, computed } from 'vue'
export default {

    props: {
        task:{
            required: true,
            limit: {
                type: Number
            },
            id: {
                type: Number
            }
        }
    },
    setup(props){
        let cid = ref(0)
        let counter = ref(0)
        let tid = ref(props.task.id)
        
        const timeRemaining = computed(() => props.task.limit - counter.value )

        function checkCount() {
            if (counter.value == props.task.limit){
            window.alert("Timer complete, nice job!")
            clearInterval(cid)
            counter = 0
            return
            }
            counter.value++;
        }

        function startTimer(event){
            console.log("Timer started" + event.target.tagName)
            cid = setInterval(checkCount, 1000)
        }

        function stopTimer() {
            console.log("timer aborted")
            window.clearInterval(cid)
            counter = 0
        }
        
        return {
            cid,
            counter,
            tid,
            checkCount,
            startTimer,
            stopTimer,
            timeRemaining
        } 
    }
}
</script>