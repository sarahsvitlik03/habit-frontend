<script setup>
import { ref, onMounted, nextTick } from "vue";

//props are passing from parent to child a chore object + formatDate
const props = defineProps({
  chore: { type: Object, required: true },
  formatDate: { type: Function, required: true },
});
// defineEmits -> allows child to notify parent when chore is updated or saved
const emit = defineEmits(["update", "save"]);

const isEditingName = ref(false);
const titleInput = ref(null);

function startEditName() {
  isEditingName.value = true;
  nextTick(() => titleInput.value?.focus());
}

onMounted(() => {
  if (!props.chore.name?.trim() || props.chore.name === "New Chore") {
    startEditName();
  }
});
</script>

<template>
  <div class="chores-card" :class="{ completed: props.chore.completed }">
    <div class="container">
      <h3>
        <input
          type="checkbox"
          v-model="props.chore.completed"
          @change="emit('update', props.chore)"
        />
        <span v-if="!isEditingName" class="chore-title">{{
          props.chore.name || "New Chore"
        }}</span>
        <input
          v-else
          ref="titleInput"
          type="text"
          v-model="props.chore.name"
          placeholder="Chore name"
        />
        <button
          v-if="!isEditingName"
          type="button"
          class="edit-btn"
          @click="startEditName"
        >
          Edit
        </button>
      </h3>

      <div class="assigned">
        <span>Assigned to</span>
        <input
          type="text"
          v-model="props.chore.assigned"
          placeholder="Unassigned"
        />
      </div>

      <input
        class="due-date"
        type="date"
        :value="props.formatDate(props.chore.day)"
        @input="props.chore.day = $event.target.value"
      />

      <p class="status-text">
        {{ props.chore.completed ? "All done" : "Still needs doing" }}
      </p>

      <div class="actions">
        <button @click="emit('save', props.chore)">Save</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.chores-card {
  background: #fff;
  border: 1px solid #ff9a76;
  border-radius: 12px;
  box-shadow: 0 6px 16px rgba(255, 127, 80, 0.18);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}
.chores-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 22px rgba(255, 127, 80, 0.28);
}

.chores-card.completed {
  background: #fafafa;
}

.container {
  padding: 1rem;
}

h3 {
  align-items: center;
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-top: 0;
}

.chore-title {
  color: #2b211d;
  flex: 1;
  font-size: 1.15rem;
  font-weight: 700;
  line-height: 1.3;
  min-width: 0;
  word-break: break-word;
}

.chores-card.completed .chore-title {
  color: #7a7a7a;
  text-decoration: line-through;
}

.edit-btn {
  background: #fff3ec;
  border: 1px solid #ffd4c2;
  border-radius: 8px;
  color: #e66b43;
  cursor: pointer;
  font-size: 0.8rem;
  font-weight: 700;
  padding: 0.35rem 0.65rem;
  transition: background-color 0.2s ease, transform 0.2s ease;
}

.edit-btn:hover {
  background: #ffe4d6;
  transform: translateY(-1px);
}

.assigned {
  margin-top: 0.4rem;
  font-size: 0.8rem;
}

.assigned span {
  display: block;
  font-size: 0.68rem;
  letter-spacing: 0.04em;
  margin-bottom: 0.25rem;
  text-transform: uppercase;
}

.assigned input,
.due-date {
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 0.82rem;
  padding: 0.3rem 0.45rem;
  width: 97%;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.due-date {
  margin-top: 0.4rem;
}

h3 input[type="text"] {
  border: 1px solid #ddd;
  border-radius: 8px;
  flex: 1;
  min-width: 0;
  padding: 0.4rem 0.5rem;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

.assigned input:focus,
.due-date:focus,
h3 input[type="text"]:focus {
  border-color: #ff7f50;
  box-shadow: 0 0 0 3px rgba(255, 127, 80, 0.12);
  outline: none;
}

.status-text {
  background: #fff3ec;
  border-radius: 999px;
  color: #c95f38;
  display: inline-block;
  font-size: 0.9rem;
  font-weight: 700;
  margin: 0.9rem 0 0;
  padding: 0.35rem 0.7rem;
}

.chores-card.completed .status-text {
  background: #e8f5e9;
  color: #2e7d32;
}

.actions {
  margin-top: 1rem;
  text-align: right;
}
.actions button {
  background-color: #ff7f50;
  border: none;
  border-radius: 8px;
  color: white;
  cursor: pointer;
  font-weight: bold;
  padding: 0.45rem 1rem;
  transition: background-color 0.3s ease, transform 0.2s ease;
}
.actions button:hover {
  background-color: #e66b43;
  transform: translateY(-1px);
}
</style>
