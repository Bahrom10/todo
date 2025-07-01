<template>
  <div class="nav_outer">
    <div v-if="nav" class="nav_field">
      <button class="nav_field_back" @click="closeModal">
        <img src="../assets/img/arrow.svg" alt="" />
      </button>
      <input type="text" class="nav_field_input" placeholder="Поиск..." v-model="search"
        @keydown.enter="find(search)" />
      <button class="nav_field_cross">
        <img src="../assets/img/exit.svg" alt="" @click="clear" />
      </button>
    </div>
    <nav v-else class="nav">
      <p class="nav_filler">.</p>
      <h2 class="nav_title">Заметки</h2>
      <button class="nav_search" @click="openModal">
        <img src="../assets/img/search.png" alt="" />
      </button>
    </nav>
  </div>
</template>

<script setup>
import { ref } from "vue";

const search = ref("");
const nav = ref(false);
const notes = ref(JSON.parse(localStorage.getItem("notes")));
const emitingMessage = ref([]);
const emit = defineEmits(["searchStr"])

const find = (str) => {
  emitingMessage.value = notes.value.filter((note) => {
    return note.title.includes(str) || note.text.includes(str);
  });
  console.log(emitingMessage.value, str);
  emit("searchStr", emitingMessage);
};

const clear = () => {
  search.value = "";
};

const openModal = () => {
  nav.value = !nav.value;
};

const closeModal = () => {
  nav.value = !nav.value;
  emit("searchStr", {
    value: []
  });
}
</script>

<style lang="scss" scoped></style>
