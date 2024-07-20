<script setup>
import { onMounted, ref, watch } from 'vue';

const newItem = ref('');
const items = ref([]);
let id = 0;

const addItem = () => {
  if (newItem.value.trim() !== '') {
    items.value.push({ id: id++, text: newItem.value.trim(), completed: false });
    newItem.value = '';
  }
};

const toggleComplete = (item) => {
  item.completed = !item.completed;
};

const removeItems = (itemID) => {
  items.value = items.value.filter((item) => item.id !== itemID);
};

onMounted(() => {
  const savedItems = localStorage.getItem('items')
  if(savedItems) {
    items.value = JSON.parse(savedItems)
  }
})

watch(items, (newItems) => {
localStorage.setItem('items', JSON.stringify(newItems))
}, { deep: true }) 

</script>

<template>
  <header>
    <h1>To do</h1>
    <div class="addNewItem">
      <input v-model="newItem" @keyup.enter="addItem" placeholder="Add Item">
      <button @click="addItem" :disabled="newItem.length === 0">Add item</button>
    </div>
  </header>

  <main>
    <ul>
      <li class="listItem" v-for="item in items" :key="item.id" :class="{completed: item.completed}" @click="toggleComplete(item)">
        <p class="listToggle" >
          {{ item.text }}
        </p>
        <button class="removeItem" @click="removeItems(item.id)">Remove item</button>
      </li>
    </ul>
  </main>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  line-height: 1.4;
}

header {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 1.5rem;
}

h1 {
  margin: 2rem;
}

.addNewItem {
  width: 100%;
  display: flex;
  justify-content: space-evenly;
}

input {
  height: 2rem;
  width: 70%;
  border: 1px solid black;
  border-radius: 4px;
  padding-left: 0.5rem;
}

.listItem {
  padding: 12px 16px;
  border: 1px solid #e8e8e8;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 6px;
  border-radius: 4px;
}

.listItem.completed {
  background-color: lightgrey;
}

.listItem.completed .listToggle {
  text-decoration: line-through;
}

button {
  padding: .4rem;
  background-color: white;
  border: 1px solid black;
  border-radius: 4px;
}

button:disabled {
  border: 1px solid rgba(16, 16, 16, 0.3);
}
</style>
