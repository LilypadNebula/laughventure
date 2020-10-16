<template>
  <div class="flex flex-col items-center">
    <p class="text-3xl">Laughventure</p>
    <div class="flex flex-col md:flex-row justify-around w-full text-lg">
      <div class="flex flex-col md:items-center md:w-1/4">
        <input type="text" v-model="firstStudent.name">
        HP
        <input type="text" class="w-16" v-model="firstStudent.hp">
        DPS
        <input type="text" class="w-16" v-model="firstStudent.dps">
        <button @click="randomizeStudent(firstStudent)">Randomize!</button>
      </div>
      <div class="text-center my-6 md:w-1/4">
        <button @click="compete">Compete!</button>
        <transition name="fade" mode="out-in">
          <p v-if="winner == ''" key="start"></p>
          <div class="text-3xl animate__animated animate__tada" v-else-if="winner != 'tie'" key="winner"><p>The winner is:</p> {{winner}}</div>
          <p class="text-3xl" v-else key="tie">It's a tie!</p>
        </transition>
      </div>
      <div class="flex flex-col md:items-center md:w-1/4">
        <input type="text" v-model="secondStudent.name">
        HP
        <input type="text" class="w-16" v-model="secondStudent.hp">
        DPS
        <input type="text" class="w-16" v-model="secondStudent.dps">
        <button @click="randomizeStudent(secondStudent)">Randomize!</button>
      </div>
    </div>
  </div>
</template>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
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
    const winner = ref('')

    const randomNum = (max, min = 0) => {
      return Math.floor(Math.random() * (Math.floor(max) - Math.ceil(min) + 1)) + Math.ceil(min)
    }

    const randomName = () => {
      let num = randomNum(nameList.value.length - 1)
      return nameList.value[num]
    }

    const compete = () => {
      const timer = winner.value == '' ? 0 : 500
      winner.value = ''
      setTimeout(() => {
        const first = Math.ceil(Math.abs(firstStudent.hp) / secondStudent.dps)
        const second = Math.ceil(Math.abs(secondStudent.hp) / firstStudent.dps)
        winner.value = first > second ? firstStudent.name : first === second ? 'tie' : secondStudent.name
      }, timer)
    }

    const randomizeStudent = (student) => {
      winner.value = ''
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

    return {firstStudent, secondStudent, winner, compete, randomizeStudent}
  }
}
</script>
