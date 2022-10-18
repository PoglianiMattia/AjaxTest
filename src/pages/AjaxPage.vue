<template>
  <q-page>
    <div class="q-pa-xl" style="max-width: 800px">
      <div class="text-h3 q-dx-md">Task</div>
      <q-form @submit="onSubmit" @reset="onReset" class="q-gutter-md">
        <q-input
          filled
          v-model="task"
          label="Enter Task"
          hint="Your task here"
          lazy-rules
          :rules="[(val) => (val && val.length > 0) || 'Please type something']"
        />

        <q-toggle v-model="accept" label="I accept the license and terms" />

        <div>
          <q-btn label="Submit" type="submit" color="primary" />

          <q-btn
            label="Reset"
            type="reset"
            color="primary"
            flat
            class="q-ml-sm"
          />
        </div>
      </q-form>
      <q-btn
        label="stampa PDF"
        @click="stampaPDF"
        color="negative"
        flat
        rounded
      />
      <div>task globale: {{ task }}</div>
      <q-list separator bordered class="bg-white">
        <q-item v-for="task in data.tasks" :key="task" clickable v-ripple>
          <q-item-section>{{ task }}</q-item-section>
          <q-item-section avatar>
            <q-icon color="primary" name="check" />
          </q-item-section>
        </q-item>
      </q-list>
    </div>
  </q-page>
</template>
<script setup>
import { ref, onMounted } from "vue";
import { useQuasar } from "quasar";
import { api } from "boot/axios";

// reactive state
const count = ref(0);
const data = ref({
  newTask: "",
  tasks: ["prova1"],
});

// functions that mutate state and trigger updates
function increment() {
  count.value++;
}

// lifecycle hooks
onMounted(() => {
  console.log(`The initial count is ${count.value}.`);
});

const $q = useQuasar();
const task = ref(null);
const accept = ref(false);

function onSubmit() {
  if (accept.value !== true) {
    $q.notify({
      color: "red-5",
      textColor: "white",
      icon: "warning",
      message: "You need to accept the license and terms first",
    });
  } else {
    console.log("task inserito", task.value);
    api
      .get("/addTask?id=" + task.value)
      .then((response) => {
        console.log("successo", response);
        console.log("task:", task.value);
        console.log("data:", data.value);
        data.value.newTask = task.value;
        data.value.tasks.push(task.value);
        console.log("data again:", data.value);

        $q.notify({
          color: "green-4",
          textColor: "white",
          icon: "cloud_done",
          message: "Submitted",
        });
      })
      .catch(() => {
        $q.notify({
          color: "negative",
          position: "top",
          message: "Loading failed",
          icon: "report_problem",
        });
      });
  }
}

function onReset() {
  task.value = null;
  accept.value = false;
  data.value = {
    newTask: "",
    tasks: [],
  };
}

function stampaPDF() {
  console.log("PDF stampato");
  return "ciao";
}
</script>
