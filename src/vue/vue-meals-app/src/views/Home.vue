<template>
  <div class="flex flex-col p-8">
    <input
      type="text"
      class="rounded border-2 border-gray-200 w-full"
      placeholder="Search for Meals"
    />
    <div class="flex justify-center gap-2 mt-2">
      <!-- :to="`/letter/${letter}`" -->
      <router-link
        v-for="(letter, index) in letters"
        :key="letter"
        :to="{ name: 'byLetter', params: { letter } }"
      >
        {{ letter }}
      </router-link>
    </div>
  </div>
</template>

<script setup>
import { ref, computed,onMounted } from 'vue'
import { useStore } from 'vuex'
import axiosClient from '../axiosClient'

const store = useStore()
const meals = computed(() => {
  return store.state.meals
})

// a-z字母
const letters = ref('ABCDEFGHIJKLMNOPQRSTXYZ'.split(''))

onMounted(async () => {
  console.log(axiosClient);
  const { data } = await axiosClient.get('list.php?i=list')
})
</script>

<style lang="scss" scoped></style>
