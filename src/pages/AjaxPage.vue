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
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import { useQuasar } from "quasar";
import { ref } from "vue";
import { api } from 'boot/axios'

export default {
  setup() {
    const $q = useQuasar();
    const task = ref(null);
    const accept = ref(false);

    return {
      task,
      accept,

      onSubmit() {
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
      },

      onReset() {
        task.value = null;
        accept.value = false;
      },
    };
  },
  methods: {
    stampaPDF() {
      console.log("PDF stampato");
      return "ciao";
    },
    addTask() {
      this.tasks.push({ task: this.task });
      this.tasks = "";
      console.log("array", this.tasks);
    },
  },
  define: {
    data() {
      return {
        newTask: "",
        tasks: [],
      };
    },
  },
};
</script>
