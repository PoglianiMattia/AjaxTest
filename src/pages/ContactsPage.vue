<template>
  <q-page class="q-pa-md">
      <q-card class="my-card">
        <q-parallax src="https://picsum.photos/1000/1000" :height="200" />

        <q-card-section>
          <div class="text-h6">Contattaci</div>
          <div class="text-subtitle2">
            Lorem ipsum dolor sit amet, consectetur adipisicing elit. Odit
            dolorem commodi beatae nulla tempore explicabo nostrum perferendis
            ab in, ratione assumenda totam dicta ipsum eveniet a magnam veniam
            temporibus sequi.
          </div>
        </q-card-section>
      </q-card>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "ContactsPage",
});
</script>

<style lang="scss">
.my-card {
  width: 100%;
  max-width: 1000px;
}
</style>
