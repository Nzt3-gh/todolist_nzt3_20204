<script setup lang="ts">
import {computed, ref} from 'vue'

interface Task{
  name: string
  finished: boolean
}

const taskList=ref<Task[]>([
  {name: 'dummy', finished: true},
  {name: "dummy2", finished: false}
])

const newTaskName= ref('')
const addTask= ()=>{
  if(newTaskName.value!=''){
    taskList.value.push({name: newTaskName.value,finished:false})
  }
  newTaskName.value=''
}

const finishedTasks=computed(()=> taskList.value.filter(x => x.finished))
const ongoingTasks=computed(()=> taskList.value.filter(x => !x.finished))

const done = (t:Task)=>{
  t.finished=true
}
</script>

<template>
<div>
  <p>タスク追加</p>
  <label>名前
    <input v-model="newTaskName" type="text">
  </label>
  <button @click="addTask">追加</button>
</div>
<div>
  TODO
  <div id="todoList" class="list">
    <div v-if="ongoingTasks.length==0">
      <p><marquee>タスクはありません</marquee></p>
    </div>
    <div v-if="ongoingTasks.length!=0">
      <ul>
      <li v-for="t in ongoingTasks" :key="t.name">
        <div>タスク名: {{ t.name }}</div>
        <div>
          <button type="button" @click="done(t)">完了</button>
        </div>
      </li>
    </ul>
    </div>
  </div>
  完了済
  <div id="finishedTaskList" class="list">
    <ul>
      <li v-for="t in finishedTasks" :key="t.name">
        <div>タスク名: {{ t.name }}</div>
      </li>
    </ul>
  </div>
</div>

</template>

<style>
.list{
  margin: 5px;
  border: solid;
}
</style>