<script lang="ts" setup>
import { useCurrentUserStore } from '@/stores/CurrentUserStore';
import { useTaskStore } from '@/stores/TaskStore';
import { computed, ref } from 'vue';
import type { TaskType } from './Task/Task';

const taskStore = useTaskStore();
const userStore = useCurrentUserStore();

const task = computed({
    get () { 
        return taskStore.selectedTask
    }, 
    set (updatedTask: TaskType) {
        taskStore.selectedTask = updatedTask
    }
});

const taskName = computed({
    get () { 
        return taskStore.selectedTask.name
    }, 
    set (name: string) {
        taskStore.selectedTask.name = name
    }
});
const taskPatient = computed({
    get () { 
        return taskStore.selectedTask.patient
    }, 
    set (patient: string) {
        taskStore.selectedTask.patient = patient
    }
});
const taskLocation = computed({
    get () { 
        return taskStore.selectedTask.location
    }, 
    set (location: string) {
        taskStore.selectedTask.location = location
    }
});
const taskDate = computed({
    get () { 
        return taskStore.selectedTask.date.toDateString()
    }, 
    set (date: string) {
        taskStore.selectedTask.date = new Date(date);
    }
});
const taskStartHr = computed({
    get () { 
        return taskStore.selectedTask.startTime.hour
    }, 
    set (start: number) {
        taskStore.selectedTask.startTime.hour = start;
    }
});
const taskStartMin = computed({
    get () { 
        return taskStore.selectedTask.startTime.minute.toString().padStart(2, '0')
    }, 
    set (start: number) {
        taskStore.selectedTask.startTime.minute = start;
    }
});
const taskEndHr = computed({
    get () { 
        return taskStore.selectedTask.endTime.hour
    }, 
    set (end: number) {
        taskStore.selectedTask.endTime.hour = end;
    }
});
const taskEndMin = computed({
    get () { 
        return taskStore.selectedTask.endTime.minute.toString().padStart(2, '0')
    }, 
    set (end: number) {
        taskStore.selectedTask.endTime.minute = end;
    }
});
const taskOwner = computed(() => { 
    return taskStore.selectedTask.owner
});
const taskStatus = computed(() => { 
    return taskStore.selectedTask.status
});


const taskModel = ref(task);

const updateSelectedTaskOwner = (isChecked: boolean, username: string) => {
    isChecked ? taskStore.updateTaskOwner(username) : taskStore.updateTaskOwner('')
}

function get(): import("vue").ComputedGetter<unknown> {
    throw new Error('Function not implemented.');
}
</script>

<template>

    <!-- TODO let a practitioner edit the description -->
    <h1 class="spacing">
        Task Description
    </h1>

    <h4>Task Name:<input v-model="taskName"></h4>

    <span>
        <div class="horizontal">
            <span ><h4>Task Owner:</h4> <h5 :class="{'claimed-task': taskOwner, 'unclaimed-task': !taskOwner}"> {{ taskOwner ? taskOwner : 'OWNER NEEDED'}} </h5></span>            
        </div>
        <p><span style="display:inline-block; width: 2rem;"></span><b>Task Status</b> <i>{{ taskStatus }} </i></p>
        <span style="display:inline-block; width: 2rem;"></span>
        
        <input 
            type="checkbox" 
            @change="updateSelectedTaskOwner($event.target?.checked, userStore.user.name)"
            :checked="task.owner === userStore.user.name"
        > Assign to me

    </span>
    
    <p><b>Patient name: </b><input v-model="taskPatient"> </p>
    <p><b>Location: </b><input v-model="taskLocation"></p>
    <p><b>Date: </b><input v-model="taskDate"></p>
    <p><b>Start hour: </b>
        <input v-model="taskStartHr" type="number" class="time-input"> 
        <b>minute: </b>
        <input v-model="taskStartMin" type="number" class="time-input"></p>
    <p><b>End hour: </b>
        <input v-model="taskEndHr" type="number" class="time-input"> 
        <b>minute: </b>
        <input v-model="taskEndMin" type="number" class="time-input"></p>

</template>

<style scoped>
.horizontal {
    display: block ruby;
}

.claimed-task {
    color: rgb(0, 123, 255);
}

.unclaimed-task {
    color: rgb(255, 38, 0);
}

.time-input {
    width: 3rem;
}

</style>