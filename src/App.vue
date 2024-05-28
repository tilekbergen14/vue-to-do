<script setup>
import { ref, computed, onMounted, watch } from "vue";

const name = ref("");
const inputContext = ref("");
const category = ref("personal");
const todoArr = ref([]);
const filteredArr = ref([]);
const createdAt = ref(null);
const edit = ref(false);

onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  todoArr.value = JSON.parse(localStorage.getItem("todos")) || [];
  filteredArr.value = todoArr.value.filter(
    (e) => e.category === category.value
  );
});

const addTodo = () => {
  if (inputContext.value.trim() === "" || category.value === null) {
    return;
  }
  if (edit.value === false) {
    todoArr.value.push({
      todo: inputContext.value,
      category: category.value,
      createdAt: new Date().getTime(),
      done: false,
    });
    inputContext.value = "";
  } else {
    todoArr.value.forEach((e) => {
      if (e.createdAt === createdAt.value) {
        (e.todo = inputContext.value),
          (e.category = category.value),
          (e.createdAt = new Date().getTime()),
          (e.done = false);
        return e;
      }
      return e;
    });
    inputContext.value = "";
    edit.value = false;
  }
};

const handleDelete = (e) => {
  todoArr.value = todoArr.value.filter((todo) => {
    return todo.createdAt !== e.createdAt;
  });
};

const handleEdit = (e) => {
  edit.value = true;
  inputContext.value = e.todo;
  category.value = e.category;
  createdAt.value = e.createdAt;
};

watch(name, (value) => {
  localStorage.setItem("name", value);
});

watch(category, (cat) => {
  filteredArr.value = todoArr.value.filter((e) => {
    return e.category === cat;
  });
});

watch(
  todoArr,
  (value) => {
    localStorage.setItem("todos", JSON.stringify(value));
    filteredArr.value = todoArr.value.filter(
      (e) => e.category === category.value
    );
  },
  { deep: true }
);
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
        name="todo"
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
        <div></div>
        <div class="category-box">
          <input type="radio" name="category" v-model="category" value="work" />
          <p>Work</p>
        </div>
      </div>

      <input
        v-if="!edit"
        type="submit"
        name="Submit"
        value="Add"
        class="button"
      />
      <input
        v-if="edit"
        type="submit"
        name="Submit"
        value="Edit"
        class="button"
      />
    </form>

    <ul>
      <li v-for="(item, index) in filteredArr" :key="index">
        <p>{{ item.todo }}</p>
        <button @click="handleEdit(item)" class="ml-4">Edit</button>
        <button @click="handleDelete(item)" class="ml-4 success">Done</button>
      </li>
    </ul>
  </div>
</template>

