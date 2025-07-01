<template>
  <main class="main">
    <Navbar @searchStr="searchRes" />
    <div class="main_div">
      <p class="main_title">Все заметки:</p>
      <button class="main_btn" @click="change">
        <img src="../assets/img/list.svg" alt="" v-if="btn" />
        <img src="../assets/img/grid.svg" alt="" v-else-if="!btn" />
        <span v-if="!btn">Сетка</span>
        <span v-else-if="btn">Список</span>
      </button>
    </div>
    <div class="main_content container">
      <div class="notes1" v-if="!notes2.length"
        :style="`${btn == false ? `grid-template-columns: 1fr` : `grid-template-columns: 1fr 1fr 1fr`}`">
        <div v-for="(item, index) in notes" class="main_item" :class="{ deactive: !item.state }" :key="index">
          <h2 class="main_item_title">
            {{ item.title }}
          </h2>
          <p class="main_item_date">
            {{ item.date + ` [${index}]` }}
          </p>
          <p class="main_item_text">
            {{ item.text }}
          </p>
          <div class="main_item_btns">
            <button class="main_item_btn" @click="edit(index)">
              <img src="../assets/img/pen.png" alt="" /> РЕДАКТИРОВАТЬ
            </button>
            <button class="main_item_btn" @click="remove(index)">
              <img src="../assets/img/trash.png" alt="" /> Удалить
            </button>
          </div>
        </div>
      </div>
      <div class="notes2" v-else-if="notes2.length"
        :style="`${btn == false ? `grid-template-columns: 1fr` : `grid-template-columns: 1fr 1fr 1fr`}`">
        <div v-for="(item, index) in notes2" class="main_item" :key="index + 1 + notes.length">
          <h2 class="main_item_title">
            {{ item.title }}
          </h2>
          <p class="main_item_date">
            {{ item.date + ` [${index}]` }}
          </p>
          <p class="main_item_text">
            {{ item.text }}
          </p>
          <div class="main_item_btns">
            <button class="main_item_btn" @click="edit(index)">
              <img src="../assets/img/pen.png" alt="" /> РЕДАКТИРОВАТЬ
            </button>
            <button class="main_item_btn" @click="remove(index)">
              <img src="../assets/img/trash.png" alt="" /> Удалить
            </button>
          </div>
        </div>
      </div>
    </div>
    <button class="main_edit_btn" @click="isUp = !isUp">
      <img src="../assets/img/pen.png" alt="" class="main_edit_img" />
    </button>
    <div :style="{ display: isUp ? 'none' : 'block' }" class="bg">
      <form action="" class="form" id="form">
        <p class="form_title">{{ tr ? "Добавить" : "Изменить" }} заметку</p>
        <input name="inp_title" @keydown.enter="save(title, text, currentIndex)" id="inp_title" type="text"
          placeholder="Title:" class="form_input" max="50" required v-model="title" />
        <textarea name="textarea" @keydown.enter="save(title, text, currentIndex)" id="textarea" cols="30" rows="1"
          class="form_text" placeholder="Content" required v-model="text"></textarea>
        <div class="form-btns">
          <button class="form_btn" type="button" @click="up">Отмена</button>
          <button class="form_btn" type="button" @click="save(title, text, currentIndex)">
            Сохранить
          </button>
        </div>
      </form>
    </div>
  </main>
</template>

<script setup>
import Navbar from "./Navbar.vue";
import { onMounted, ref } from "vue";

const notes = ref([]);
const btn = ref(true);
const tr = ref(true);
const currentIndex = ref(null);
// const emit = defineEmits('openSearch')
const notes2 = ref([])

const change = () => {
  btn.value = !btn.value;
};

const title = ref("");
const text = ref("");


console.log(notes.value);

const remove = (i) => {
  let localNotes = JSON.parse(localStorage.getItem("notes"));
  localNotes.splice(i, 1);
  localStorage.setItem("notes", JSON.stringify(localNotes));
  notes.value = localNotes;
};

const edit = (i) => {
  tr.value = false;
  currentIndex.value = i;
  isUp.value = !isUp.value;
  title.value = notes.value[i].title;
  text.value = notes.value[i].text;
};

const isUp = ref(true);
const up = () => {
  isUp.value = !isUp.value;
  tr.value = true;
  currentIndex.value = null;
  title.value = "";
  text.value = "";
};

const save = (titles, texts, i) => {
  if (titles !== "" && texts !== "") {
    if (i === null) {
      let nots = JSON.parse(localStorage.getItem("notes")) || [];
      const date = new Date();
      nots.push({
        title: titles,
        text: texts,
        state: true,
        date: date.toLocaleDateString(),
      });
      localStorage.setItem("notes", JSON.stringify(nots));
      notes.value = nots;
    } else {
      notes.value[i].title = titles;
      notes.value[i].text = texts;
      localStorage.setItem("notes", JSON.stringify(notes.value));
    }
    isUp.value = !isUp.value;
    title.value = "";
    text.value = "";
    currentIndex.value = null;
  }
};

const searchRes = (emitingMessage) => {
  console.log(emitingMessage.value, 'val');
  notes2.value = emitingMessage.value;
}

onMounted(() => {
  notes.value = JSON.parse(localStorage.getItem("notes"));
})
</script>

<style lang="scss" scoped></style>
