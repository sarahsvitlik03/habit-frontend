<script setup>
import { ref, onMounted } from "vue";
import ChoreCard from "./ChoreCard.vue"


/* Hard coded JSON Data -> Now pulling instead from MongoDB  
  const chores = ref([
  { name: "Vacuuming", assigned: "Sarah", day: "Saturday", completed: false },
  { name: "Dishes", assigned: "Charlie", day: "Sunday", completed: false },
  { name: "Laundry", assigned: "Mom", day: "Monday", completed: true }
]);  */

/**
 * Reactive state variables
 */
const chores = ref([]); //stores the list of chores
const loading = ref(true); // Shows "Loading..." while fetching
const error = ref(null); // Stores any error message

// fetchChores -> gets the latest chores from MongoDB
async function fetchChores() {
  const res = await fetch("https://habit-backend-s2sq.onrender.com/api/chores");
  if (!res.ok) throw new Error("Failed to fetch");
  chores.value = await res.json();
}

// saveChore -> sends PUT request to backend, then re-fetches all chores to refresh UI
async function saveChore(chore) {
  try {
    const choreId = chore._id || chore.id;
    const res = await fetch(`https://habit-backend-s2sq.onrender.com/api/chores/${choreId}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(chore),
    });
    if (!res.ok) throw new Error("Failed to save");
    await fetchChores();
  } catch (err) {
    console.error(err);
    error.value = "Could not save chore";
  }
}

// Format date for MongoDB
function formatDate(dateStr) {
  return dateStr?.slice(0, 10);  
}

//Add a new chore -> sends post request to create new chore in MongoDB, adds new chore to chores array
async function addChore() {
  try {
    const res = await fetch("https://habit-backend-s2sq.onrender.com/api/chores", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        name: "New Chore",
        assigned: "",
        day: "",
        completed: false,
      }),
    });
    if (!res.ok) throw new Error("Failed to add chore");
    await fetchChores();
  } catch (err) {
    error.value = "Could not add chore";
  }
}

//runs when component is loaded, fetches all chores, handles errors
 onMounted(async () => {
  try {
    await fetchChores();
  } catch (err) {
    console.error(err);
    error.value = "Could not load data from backend"; // Show error message
  } finally {
    loading.value = false; // Hide loading state no matter what
  }
});
</script>

<template>
  <div class="page-container">
  <div class="top-line">
    <h1>Household Chore Tracker</h1>
    <button type="button" @click="addChore">Insert Chore</button>
  </div>
  <!-- When loading -->
    <p v-if="loading" class="status">Loading data...</p>
    <!-- When an error occurs -->
    <p v-if="error" class="error">{{ error }}</p>
      <div class="chores-grid">
  <ChoreCard
    v-for="chore in chores"
    :key="chore._id"
    :chore="chore"
    :format-date="formatDate"
    @save="saveChore"
  />
  </div>
 </div>
</template>

<style scoped>
:global(body) {
  margin: 0;
  background: #fff8f4;
}

.page-container {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  margin: 5% 10%;
  color: #333;
}

.top-line {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 3px solid #ff8a65;
  padding-bottom: 0.75rem;
}

.top-line h1 {
  color: #e66b43;
  font-size: 2rem;
  margin: 0;
}

.top-line button {
  background-color: #ff7f50;
  color: white;
  border: none;
  padding: 0.7rem 1.2rem;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(255,127,80,0.25);
  transition: background-color 0.3s ease, transform 0.2s ease;
}
.top-line button:hover {
  background-color: #e66b43;
  transform: translateY(-1px);  
}

.status, .error {
  margin-top: 1rem;
  font-weight: bold;
}
.error {
  color: crimson;
}

.chores-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
  margin-top: 1.5rem;
}


.container {
  padding: 1rem;
}

h3 {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

input[type="text"], input[type="date"] {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 0.5rem;
  width: 100%;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}
input[type="text"]:focus, input[type="date"]:focus {
  border-color: #ff7f50;
  box-shadow: 0 0 0 3px rgba(255,127,80,0.12);
  outline: none;
}

.assigned {
  margin-top: 0.5rem;
  font-size: 0.9rem;
}
.assigned span {
  font-weight: bold;
  margin-right: 0.5rem;
  color: #e66b43;
}

.actions {
  margin-top: 1rem;
  text-align: right;
}
.actions button {
  background-color: #ff7f50;
  color: white;
  border: none;
  padding: 0.45rem 1rem;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}
.actions button:hover {
  background-color: #e66b43;
  transform: translateY(-1px);
}

@media (max-width: 900px) {
  .chores-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 640px) {
  .page-container {
    margin: 1.5rem;
  }

  .top-line {
    align-items: stretch;
    flex-direction: column;
    gap: 1rem;
  }

  .chores-grid {
    grid-template-columns: 1fr;
  }
}
</style>
