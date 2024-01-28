<script setup>
import {computed, onMounted, ref} from "vue";
import AnimatedPlaceholder from "@/components/AnimatedPlaceholder.vue";

const userData = ref(null)
const loading = ref(true)

const getUser = async (isInitial = false) => {
  if (isInitial) {
    loading.value = true
    const res = await fetch('https://randomuser.me/api')
    const {results} = await res.json()
    userData.value = results[0]
    setTimeout(() => {
      loading.value = false
    }, 500)
  } else {
    if (!loading.value) {
      loading.value = true
      const res = await fetch('https://randomuser.me/api')
      const {results} = await res.json()
      userData.value = results[0]
      loading.value = false
    }
  }
}

onMounted(() => {
  getUser(true)
})

const fullName = computed(() => {
  return `${userData.value.name.first} ${userData.value.name.last}`
})

const picture = computed(() => {
  return userData.value.picture.large
})

document.body.onkeyup = (e) => {
  if (e.key === " " || e.code === "Space" || e.keyCode === 32) {
    getUser()
  }
}
</script>

<template>
<div class="random-user-generator" @keyup.enter="getUser">
    <div v-if="!loading" class="user-info">
      <img v-if="picture" loading="lazy" :class="userData.gender" :src="picture" :alt="fullName">
      <h1>{{ fullName }}</h1>
      <h1>{{ userData.email }}</h1>
      <button :class="userData.gender" @click="getUser">Get Random User</button>
    </div>
  <div v-else class="loader">
    <AnimatedPlaceholder class="image-loader"/>
    <AnimatedPlaceholder class="text-loader"/>
    <AnimatedPlaceholder class="text-loader-2"/>
    <AnimatedPlaceholder class="button-loader"/>
  </div>
</div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  margin: 0;
  width: 100%;
  height: 100dvh;
  font-family: "Inter", sans-serif;
  background-color: #f4f4f4;
}

#app {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.random-user-generator, .loader {
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  flex-direction: column;
}

h1 {
  font-size: 28px;
  line-height: 30px;
  margin-bottom: 1rem;
  font-weight: 400;
}

img {
  border-radius: 50%;
  border: 5px #333 solid;
  margin-bottom: 1rem;
}

.male {
  border-color: steelblue;
  background-color: steelblue;
}

.female {
  border-color: pink;
  background-color: pink;
  color: #333;
}

button {
  cursor: pointer;
  display: inline-block;
  background: #333;
  color: white;
  font-size: 18px;
  border: 0;
  padding: 1rem 1.5rem;
  font-weight: 400;
  border-radius: 4px;
  transition: .2s;
}

button:focus {
  outline: none;
}

button:hover {
  transform: scale(0.98);
}

.loader .image-loader {
  width: 138px;
  height: 138px;
  border-radius: 50%;
  margin-bottom: 1rem;
}

.loader .text-loader, .loader .text-loader-2 {
  width: 240px;
  height: 30px;
  border-radius: 4px;
  margin-bottom: 1rem;
}

.loader .text-loader-2 {
  width: 350px;
}

.loader .button-loader {
  width: 193px;
  height: 53px;
  border-radius: 4px;
}

@media screen and (max-width: 768px) {
  img {
    width: 100px;
    height: 100px;
    margin-bottom: 12px;
  }

  h1 {
    font-size: 16px;
    line-height: 20px;
    margin-bottom: 10px;
  }

  button {
    font-size: 14px;
    padding: 10px 16px;
  }

  .loader .image-loader {
    width: 100px;
    height: 100px;
    margin-bottom: 12px;
  }

  .loader .text-loader, .loader .text-loader-2 {
    width: 150px;
    height: 20px;
    margin-bottom: 10px;
  }

  .loader .text-loader-2 {
    width: 230px;
  }

  .loader .button-loader {
    width: 144px;
    height: 36px;
  }
}
</style>
