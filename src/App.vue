<template>
  <div class="flex flex-col items-center">
    <p class="text-3xl">Laughventure</p>
    <div class="flex justify-around w-full">
      <div class="flex flex-col items-center">
        <p>{{firstStudent.name}}</p>
        <p>HP: {{firstStudent.hp}}</p>
        <p>DPS: {{firstStudent.dps}}</p>
      </div>
      <div>Winner</div>
      <div class="flex flex-col items-center">
        <p>{{secondStudent.name}}</p>
        <p>HP: {{secondStudent.hp}}</p>
        <p>DPS: {{secondStudent.dps}}</p>
      </div>
    </div>
  </div>
</template>

<style>
body{
  @apply bg-blue-200;
}
</style>

<script>
import {onMounted, ref, reactive} from 'vue'

export default {
  name: 'Laughventure',
  setup(){
    const nameList = ref([])
    const firstStudent = reactive({name: '', hp: 0, dps: 0})
    const secondStudent = reactive({name: '', hp: 0, dps: 0})

    const randomNum = (max, min = 0) => {
      return Math.floor(Math.random() * Math.floor(max)) + min
    }

    const randomName = () => {
      let num = randomNum(nameList.value.length)
      return nameList.value[num]
    }

    const randomizeStudent = (student) => {
      student.name = randomName()
      student.hp = randomNum(50, 10)
      student.dps = randomNum(8,3)
    }

    onMounted(async () => {
      const res = await fetch('https://jsonplaceholder.typicode.com/users')
      const users = await res.json()
      nameList.value = users.map(user => user.name)
      randomizeStudent(firstStudent)
      randomizeStudent(secondStudent)
    })

    return {firstStudent, secondStudent}
  }
}
</script>
