<script setup>
import { ref, onMounted, watch } from 'vue';

const showForm = ref(false);
const newMemo = ref("");
const memos = ref([]);
const errorMessage = ref("");

// Load memos from localStorage when component is mounted
onMounted(() => {
  const storedMemos = localStorage.getItem("memos");
  if (storedMemos) {
    memos.value = JSON.parse(storedMemos);
  }
});

// Watch for changes in memos and update localStorage
watch(memos, (newMemos) => {
  localStorage.setItem("memos", JSON.stringify(newMemos));
}, { deep: true });

function addMemo() {
  if (!newMemo.value) {
    errorMessage.value = "Please enter a memo"
    return
  }
  memos.value.push({
    id: Date.now(),
    content: newMemo.value,
    date: new Date().toLocaleDateString(),
  })
  newMemo.value = ""
  showForm.value = false
}

function deleteMemo(id) {
  memos.value = memos.value.filter((memo) => memo.id !== id)
}

</script>

<template>
  <main>
    <div class="container">
      <header>
        <h1 class="header-title">Memo App</h1>
        <button @click="showForm = true" class="header-button">+</button>
      </header>
      <div class="card-container">
        <div v-for="memo in memos" key="memo.id" class="card">
          <p class="card-content">
            {{ memo.content }}
          </p>
          <div class="memo-footer">
            <p>{{memo.date}}</p>
            <button @click="deleteMemo(memo.id)" class="delete-btn">Delete</button>
          </div>        
        </div>
      </div>
    </div>
    <div v-if="showForm" class="form-overlay">
      <div class="form-modal">
        <button @click="showForm = false" class="form-close-btn">&times;</button>
        <p class="form-error" v-if="errorMessage">{{ errorMessage }}</p>
        <textarea v-model="newMemo" name="memo" id="memo" cols="30" rows="10"></textarea>
        <button @click="addMemo" class="form-save-btn">Save</button>
      </div>
    </div>

    <footer>
      <p>Created by <a href="">Muhammad Vembi Yusuf Saputra</a></p>"
    </footer>
  </main>
</template>

<style scoped>
main {
  min-height: 100vh;
  min-width: 100vw;
}

.container {
  max-width: 900px;
  padding: 10px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-title {
  font-size: 48px;
  font-weight: bold;
  margin-bottom: 20px;
  color: aqua;
}

.header-button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  border-radius: 100%;
  background-color: bisque;
  font-size: 20px;
  color: white;
  cursor: pointer;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.card {
  width: 225px;
  height: 225px;
  padding: 25px;
  border-radius: 20px;
  background-color: beige;
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
  flex-direction: column;
}

.form-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: center;
}

.form-modal {
  width: 420px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.form-save-btn {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: #495a7d;
  border: none;
  cursor: pointer;
  border-radius: 5px;
  margin-top: 15px;
  color: white;
}

.form-close-btn {
  position: absolute;
  top: 5px;
  right: 10px;
  width: 30px;
  height: 30px;
  background-color: transparent;
  border: none;
  font-size: 25px;
  cursor: pointer;
}

.form-error {
  color: red;
}

.memo-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.delete-btn {
  padding: 5px 10px;
  background-color: #ff4d4d;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

footer {
  text-align: center;
  background-color: #495a7d;
  color: white;
  padding: 15px 0;
  position: fixed;
  width: 100%;
  bottom: 0;
  left: 0;
}

footer a {
  color: white;
  text-decoration: none;
}

/* Buat tampilan lebih responsif di Android */
@media screen and (max-width: 768px) {
  .container {
    width: 100%;
    padding: 15px;
  }

  .header-title {
    font-size: 32px;
  }

  .header-button {
    width: 40px;
    height: 40px;
    font-size: 18px;
  }

  .card-container {
    flex-direction: column;
    align-items: center;
  }

  .card {
    width: 90%;
    height: auto;
    padding: 15px;
  }

  .form-modal {
    width: 90%;
    max-width: 350px;
    padding: 20px;
  }

  .form-save-btn, .delete-btn {
    font-size: 16px;
    padding: 10px;
  }

  textarea {
    font-size: 16px;
    height: 120px;
  }

  footer {
    font-size: 14px;
  }
}

/* Pastikan tombol mudah ditekan di Android */
button {
  touch-action: manipulation;
}

/* Tambahkan smooth scrolling */
html {
  scroll-behavior: smooth;
}

</style>