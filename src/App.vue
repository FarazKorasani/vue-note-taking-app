<script setup>
import { ref } from 'vue';

// Handle the visibility of the overlay
const overlay = ref(false);

const toggleOverlayTrue = () => {
  overlay.value = true;
}

const toggleOverlayFalse = () => {
  overlay.value = false;
  errorMessage.value = "";
  textareaNotes.value = "";
}


// Create two-way binding between the textarea input and our data 
const textareaNotes = ref("")


// When user clicks on add note, create a new card, close the overlay, and empty the textarea
const notes = ref([])
const errorMessage = ref("")


const getRandomColor = () => {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (textareaNotes.value.length < 10) {
    errorMessage.value = "Note needs to be minimum 10 characters! Try again!";
    document.querySelector('.add-note-button').disabled = true;
    document.querySelector("#note").readOnly = true;
    textareaNotes.value = "";
    return;
  }

  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: textareaNotes.value,
    date: new Date(),
    backgroundColor: getRandomColor(),
  });

  toggleOverlayFalse();
  textareaNotes.value = "";
  errorMessage.value = "";
}

</script>

<template>
  <main>
    <div v-if="overlay" class="overlay">
      <div class="modal">
        <textarea v-model.trim="textareaNotes" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
        <div class="button-container">
          <button @click="addNote" class="add-note-button">Add</button>
          <button @click="toggleOverlayFalse" class="close-button"><span> Close</span></button>
        </div>
      </div>
    </div>
    <div class="container">
      <header>
        <h1 class="header-h1"> See Your Thoughts...</h1>
        <button @click="toggleOverlayTrue" class="header-button"> <span class="button-plus-sign">+</span></button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{ backgroundColor: note.backgroundColor }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("en-AU") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  height: 100vh;
  width: 100vw;
  background-color: lightcyan;
  overflow-y: auto;
  overflow-x: hidden;
}

/* ----------OVERLAY--------- */
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  transform: translate(-50%, -50%);
  top: 50%;
  left: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}


.modal {
  width: 850px;
  background-color: rgb(224, 224, 219);
  border-radius: 30px;
  padding: 90px;
  position: relative;
  display: flex;
  flex-direction: column;
}


textarea {
  padding: 15px;
  font-size: 18px;
  border: 2px solid #ccc;
  border-radius: 10px;
  resize: none;
  font-family: Arial, sans-serif;
  outline: none;
}

textarea:focus {
  border-color: #007bff;
}

.error {
  color: #dc3545;
  font-size: 18px;
  margin: 10px auto;
  text-align: center;
  font-weight: bold;
  padding: 15px 0;
  border: 2px solid #dc3545;
  border-radius: 5px;
  background-color: #f8d7da;
  width: 50%;
}

.button-container {
  display: flex;
  justify-content: center;
  gap: 25px;
  margin-top: 30px;
}

.add-note-button,
.close-button {
  width: 140px;
  height: 50px;
  border-radius: 5px;
  border: 1px solid black;
  font-size: 20px;
  background-color: #000000;
  border: none;
  color: rgb(255, 255, 255);
  cursor: pointer;
  transition: transform 0.3s ease;
}

.add-note-button:hover,
.close-button:hover {
  transform: scale(1.1);
}


.add-note-button:disabled {
  cursor: not-allowed;
  background-color: #cccccc;
  color: #999999;
}


/* ----------Container-------- */
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

header {
  display: flex;
  flex-direction: column;
  height: 60vh;
  justify-content: space-around;
  align-items: center;
}

.header-h1 {
  font-style: italic;
  font-size: 95px;
}

/* Define the keyframes for the following button's shining effect */
@keyframes glitter {
  0% {
    background-position: 0% 50%;
  }

  50% {
    background-position: 100% 50%;
  }

  100% {
    background-position: 0% 50%;
  }
}


.header-button {
  background-image: radial-gradient(circle, #004d9e, #0063c6, #e9eff9, #ffffff, #d8c596, #94c11f, #004d9e);
  background-size: 200% 100%;
  animation: glitter 6s infinite alternate;
  border: none;
  padding: 10px 20px;
  color: white;
  font-size: 16px;
  cursor: pointer;
  border-radius: 5px;
  outline: none;
  border: none;
  padding: 10px;
  width: 450px;
  height: 450px;
  cursor: pointer;
  border-radius: 1000px;
  color: white;
  font-size: 160px;
  transition: transform 0.3s ease;
}

.header-button:hover {
  transform: scale(1.1);
}

.button-plus-sign {
  color: black;
}


.cards-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);

  margin-top: 10px;
}

.card {
  width: 300px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-bottom: 20px;
}

.main-text {
  line-height: 1.25;
  font-size: 17.5px;
  text-align: center;
  margin: 5px;
  word-wrap: break-word;
}

.date {
  font-size: 11px;
  margin: 5px;
  text-align: center;
}

/* ---------Tablets and Larger Devices-------- */

@media screen and (min-width: 769px) and (max-width: 1920px) {

  header {
    height: 90vh;
  }

  .header-button {
    width: 22vw;

  }

  .header-h1 {
    font-size: 65px;
  }
}


/* ---------Mobile and Small Devices-------- */
@media screen and (max-width: 768px) {
  header {
    margin-top: 20px;
    height: 40vh;
    justify-content: start;
    gap: 10px;
  }

  .header-h1 {
    font-size: 30px;
  }

  .header-button {
    width: 250px;
    height: 250px;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10;
  }

  .modal {
    width: 50vw;
    padding: 50px;
  }

  .button-container {
    margin-top: 0px;
    gap: 10px;
  }

  .add-note-button,
  .close-button {
    height: 20px;
    width: 60px;
    font-size: 15px;
    border-radius: 20%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 10px;
    padding: 20px;
  }

  .error {
    width: 100%;
    font-size: 13px;

  }

  .cards-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .card {
    width: 60%;
    height: 225px;
    border-radius: 15px;
    margin-right: 0px;
  }
}
</style>