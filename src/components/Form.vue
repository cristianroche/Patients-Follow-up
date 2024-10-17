<script setup>
import { reactive, computed } from "vue";
import Alert from "./Alert.vue";

const props = defineProps({
  id: {
    type: [String, null],
    required: false,
  },
  name: {
    type: String,
    required: true,
  },
  owner: {
    type: String,
    required: true,
  },
  email: {
    type: String,
    required: true,
  },
  date: {
    type: String,
    required: true,
  },
  symptoms: {
    type: String,
    required: true,
  },
});

const emit = defineEmits([
  "update:name",
  "update:owner",
  "update:email",
  "update:date",
  "update:symptoms",
  "add-patient",
]);

const alert = reactive({
  type: "",
  message: "",
});

const isEditing = computed(() => props.id !== null);

const validate = () => {
  if (Object.values(props).includes("")) {
    alert.type = "error";
    alert.message = "All fields are required";
    return;
  }

  emit("add-patient");

  alert.type = "success";
  alert.message = "Patient added successfully";

  setTimeout(() => {
    alert.message = "";
    alert.type = "";
  }, 3000);
};
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">patient follow-up</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Add a new patient and
      <span class="text-indigo-600 font-bold">adminster</span>
    </p>

    <Alert v-if="alert.message" :alert="alert" />

    <form
      class="bg-white shadow-md rounded-lg py-10 px-5 mb-10"
      @submit.prevent="validate"
    >
      <div class="mb-5">
        <label for="pet" class="block text-gray-700 uppercase font-bold mb-2">
          Pet Name
        </label>
        <input
          type="text"
          id="pet"
          placeholder="Pet Name"
          class="border-2 p-2 mt-2 placeholder-gray-400 w-full rounded-md"
          :value="name"
          @input="$emit('update:name', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="owner" class="block text-gray-700 uppercase font-bold mb-2">
          Owner Name
        </label>
        <input
          type="text"
          id="owner"
          placeholder="Owner Name"
          class="border-2 p-2 mt-2 placeholder-gray-400 w-full rounded-md"
          :value="owner"
          @input="$emit('update:owner', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="email" class="block text-gray-700 uppercase font-bold mb-2">
          Owner Email
        </label>
        <input
          type="email"
          id="email"
          placeholder="Owner Email"
          class="border-2 p-2 mt-2 placeholder-gray-400 w-full rounded-md"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label for="date" class="block text-gray-700 uppercase font-bold mb-2">
          Discharge Date
        </label>
        <input
          type="date"
          id="date"
          class="border-2 p-2 mt-2 placeholder-gray-400 w-full rounded-md"
          :value="date"
          @input="$emit('update:date', $event.target.value)"
        />
      </div>
      <div class="mb-5">
        <label
          for="symptoms"
          class="block text-gray-700 uppercase font-bold mb-2"
        >
          Symptoms
        </label>
        <textarea
          id="symptoms"
          placeholder="Describe the symptoms"
          class="border-2 p-2 mt-2 placeholder-gray-400 w-full rounded-md h-40"
          :value="symptoms"
          @input="$emit('update:symptoms', $event.target.value)"
        />
      </div>

      <input
        type="submit"
        :value="[isEditing ? 'Edit Patient' : 'Add Patient']"
        class="bg-indigo-600 hover:bg-indigo-700 text-white uppercase font-bold py-2 px-4 rounded-md cursor-pointer transition-colors duration-300 w-full"
      />
    </form>
  </div>
</template>
