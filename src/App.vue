<script setup>
import { ref } from 'vue';
const showModal = ref(false);
const text = ref("");
const errorMessage = ref("");
const newNotes = ref([]);

function getRandomColor() {
  const color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  return color;
}

const addNote = () =>{
  if (text.value.length < 10) {
    return errorMessage.value = "Note needs to be 10 caracters or more"
  }
  newNotes.value.push({
    text: text.value,
    date: new Date(),
    id: Math.floor(Math.random() * 100000),
    backgroundColor: getRandomColor(),
  });
  showModal.value = false;
  text.value = "";
  errorMessage.value = "";
}
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
    <div class="modal">
      <textarea v-model.trim="text" name="note" id="" cols="30" rows="10"></textarea>
      <p v-if="errorMessage">{{ errorMessage }}</p>
      <button @click="addNote">Add Note</button>
      <button class="close" @click="showModal = false">Close</button>
    </div>
  </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div 
          v-for="newNote in newNotes"
          :key="newNote.id" 
          class="card" :style="{backgroundColor: newNote.backgroundColor}">
          <p class="main-text">{{ newNote.text }}</p>
          <p class="date">{{ newNote.date.toLocaleDateString("en-ES") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 75px;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  border-radius: 100%;
  color: white;
  background-color: black;
  font-size: 20px;
}

.card {
  width: 225px;
  height: 225px;
  background-color: blueviolet;
  padding: 10px;
  border-radius: 15%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
}
.date {
  font-size: 12px;
  font-weight: bold;
}
.cards-container {
  display: flex;
  flex-wrap: wrap;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(0, 0, 0, 0.7777);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  justify-content: center;
  flex-direction: column;
}
.modal button {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  cursor: pointer;
  margin-top: 10px;
}
.modal .close {
  margin-top: 7px;
  background-color: red;
}
.modal p {
  color: red;
}
</style>