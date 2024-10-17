<script setup>
import { ref, reactive, watch, onMounted } from "vue";

import Header from "./components/Header.vue";
import Form from "./components/Form.vue";
import Patient from "./components/Patient.vue";

import { uid } from "uid";

const patients = ref([]);

const patient = reactive({
  id: null,
  name: "",
  owner: "",
  email: "",
  date: "",
  symptoms: "",
});

watch(
  patients,
  () => {
    AddToStorage();
  },
  { deep: true }
);

const AddToStorage = () => {
  localStorage.setItem("patients", JSON.stringify(patients.value));
};

onMounted(() => {
  const patientsLS = localStorage.getItem("patients");
  if (patientsLS) {
    patients.value = JSON.parse(patientsLS);
  }
});

const addPatient = () => {
  if (patient.id) {
    const index = patients.value.findIndex((p) => p.id === patient.id);
    patients.value[index] = { ...patient };
  } else {
    patients.value.push({ ...patient, id: uid() });
  }

  Object.assign(patient, {
    id: null,
    name: "",
    owner: "",
    email: "",
    date: "",
    symptoms: "",
  });
};

const editPatient = (id) => {
  const patientToEdit = patients.value.find((patient) => patient.id === id);

  Object.assign(patient, patientToEdit);
};

const deletePatient = (id) => {
  patients.value = patients.value.filter((patient) => patient.id !== id);
};
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />
    <div class="mt-12 md:flex">
      <Form
        v-model:name="patient.name"
        v-model:owner="patient.owner"
        v-model:email="patient.email"
        v-model:date="patient.date"
        v-model:symptoms="patient.symptoms"
        @add-patient="addPatient"
        :id="patient.id"
      />
      <div class="md:w-1/2 md:h-screen overflow-y-scroll">
        <h2 class="font-black text-3xl text-center">manage your patients</h2>

        <div v-if="patients.length > 0">
          <p class="text-lg mt-5 text-center mb-10">
            <span class="text-indigo-600 font-bold">Patients</span> Information
          </p>
          <Patient
            v-for="patient in patients"
            :patient="patient"
            @edit-patient="editPatient"
            @delete-patient="deletePatient"
          />
        </div>

        <p v-else class="mt-20 text-2xl text-center">There are no patients</p>
      </div>
    </div>
  </div>
</template>
