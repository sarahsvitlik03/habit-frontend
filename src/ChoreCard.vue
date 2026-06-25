<script setup>
//props are passing from parent to child a chore object + formatDate
const props = defineProps({  
  chore: { type: Object, required: true },
  formatDate: { type: Function, required: true }
})
// defineEmits -> allows child to notify parent when chore is updated or saved
const emit = defineEmits(["update", "save"])  
</script>

<template>
  <div class="chores-card">
    <div class="container">
      <h3>
        <input
          type="checkbox"
          v-model="props.chore.completed"
          @change="emit('update', props.chore)"
        />
        <input
          type="text"
          v-model="props.chore.name"
          placeholder="Chore name"
        />
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
        type="date"
        :value="props.formatDate(props.chore.day)"
        @input="props.chore.day = $event.target.value"
      />

      <p class="status-text">{{ props.chore.completed ? "All done" : "Still needs doing" }}</p>

      <div class="actions">
        <button @click="emit('save', props.chore)">Save</button> <!-- Emit? -->
      </div>
    </div>
  </div>
</template>

<style scoped>
.chores-card {
  background: #fff;
  border: 1px solid #ff9a76;
  border-radius: 10px;
  box-shadow: 0 6px 16px rgba(255,127,80,0.18);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}
.chores-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 10px 22px rgba(255,127,80,0.28);
}

.container {
  padding: 1rem;
}

h3 {
  margin-top: 0;
}

.assigned span {
  display: block;
  font-size: 0.8rem;
  letter-spacing: 0.04em;
  margin-bottom: 0.35rem;
  text-transform: uppercase;
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
</style>
