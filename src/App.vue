<script setup>
import { ref, computed, onMounted, watch } from "vue";

const name = ref("");
const inputContext = ref("");
const category = ref(null);
const todoArr = ref([]);

watch(name, (value) => {
  localStorage.setItem("name", value);
});

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
});

const addTodo = () => {
  if (inputContext.value.trim() === "" || category.value === null) {
  } else {
    todoArr.value.push({
      todo: inputContext.value,
      category: category.value,
      createdAt: new Date().getTime(),
    });
    inputContext.value = "";
    category.value = null;
  }
};
</script>

<template>
  <div class="container">
    <header>
      <h3>
        Hello,
        <input
          placeholder="your name"
          class="disabled name-input"
          v-model="name"
        />
      </h3>
    </header>
    <form @submit.prevent="addTodo" class="main-section">
      <p>Create a to do!</p>
      <input
        type="text"
        name=""
        v-model="inputContext"
        class="input"
        placeholder="Start writing..."
      />
      <div class="category">
        <div class="category-box">
          <input
            type="radio"
            name="category"
            v-model="category"
            value="personal"
          />
          <p>Personal</p>
        </div>
        <div class="category-box">
          <input type="radio" name="category" v-model="category" value="work" />
          <p>Work</p>
        </div>
      </div>
      <input type="submit" name="Submit" value="Add" class="button" />
    </form>

    <ul>
      <li v-for="(item, index) in todoArr" :key="index">
        <p>{{ item.todo }}</p>
        <button class="ml-4">Edit</button>
        <button class="ml-4 success">Done</button>
      </li>
    </ul>
  </div>
</template>

