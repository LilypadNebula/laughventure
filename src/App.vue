<template>
  <div class="flex flex-col items-center">
    <p class="text-3xl">Laughventure</p>
    <div class="flex justify-around w-full text-lg">
      <div class="flex flex-col items-center w-1/4">
        <input type="text" v-model="firstStudent.name">
        HP
        <input type="text" class="w-16" v-model="firstStudent.hp">
        DPS
        <input type="text" class="w-16" v-model="firstStudent.dps">
      </div>
      <div class="text-center w-1/4">
        <button @click="compete">Compete!</button>
        <p class="text-xl">{{winner}}</p>
      </div>
      <div class="flex flex-col items-center w-1/4">
        <input type="text" v-model="secondStudent.name">
        HP
        <input type="text" class="w-16" v-model="secondStudent.hp">
        DPS
        <input type="text" class="w-16" v-model="secondStudent.dps">
      </div>
    </div>
  </div>
</template>

<script>
import {onMounted, ref, reactive} from 'vue'

export default {
  name: 'Laughventure',
  setup(){
    const nameList = ref([])
    const firstStudent = reactive({name: '', hp: 0, dps: 0})
    const secondStudent = reactive({name: '', hp: 0, dps: 0})
    const winner = ref('')

    const randomNum = (max, min = 0) => {
      return Math.floor(Math.random() * (Math.floor(max) - Math.ceil(min) + 1)) + Math.ceil(min)
    }

    const randomName = () => {
      let num = randomNum(nameList.value.length - 1)
      return nameList.value[num]
    }

    const compete = () => {
      const first = Math.ceil(Math.abs(firstStudent.hp) / secondStudent.dps)
      const second = Math.ceil(Math.abs(secondStudent.hp) / firstStudent.dps)
      winner.value = first > second ? firstStudent.name : first === second ? 'Tie' : secondStudent.name
    }

    const randomizeStudent = (student) => {
      student.name = randomName()
      student.hp = randomNum(-50, -125)
      student.dps = randomNum(9, 5)
    }

    onMounted(async () => {
      const res = await fetch('https://jsonplaceholder.typicode.com/users')
      const users = await res.json()
      nameList.value = users.map(user => user.name)
      randomizeStudent(firstStudent)
      randomizeStudent(secondStudent)
    })

    return {firstStudent, secondStudent, winner, compete}
  }
}
</script>
