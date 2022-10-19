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
        <q-item v-for="task in data.tasks" :key="task" v-ripple>
          <q-item-section>{{ task }}</q-item-section>
          <q-btn
            flat
            rounded
            color="primary"
            icon="check"
            v-on:click="deleteTask(task)"
          />
        </q-item>
      </q-list>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { useQuasar } from "quasar";
import { api } from "boot/axios";

const $q = useQuasar();
const task = ref(null);
const accept = ref(false);
const data = ref({
  newTask: "",
  tasks: ["prova1"],
});

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

function deleteTask(toDelete) {
  $q.dialog({
    title: "Sure?",
    message: "vuoi cancellare",
    cancel: true,
    persistent: true,
  }).onOk(() => {
    data.value.tasks = data.value.tasks.filter((item) => item !== toDelete);
    $q.notify({
      color: "green-4",
      textColor: "white",
      icon: "cloud_done",
      message: "Task deleted",
    });
  });
}

function stampaPDF() {
  console.log("PDF stampato");
  return "ciao";
}
</script>
