<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar>
        <q-img
          src="https://picsum.photos/400/100"
          class="absolute-top headerimg"
        />
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
        <div class="q-px-lg q-pt-xl q-mb-md">
          <div class="text-h3">Ajax app</div>
          <div class="text-subtitle1">{{ today }}</div>
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="200"
      :breakpoint="1500"
    >
      <q-scroll-area class="fit">
        <q-list padding class="menu-list">
          <q-item clickable v-ripple to="/" exact>
            <q-item-section avatar>
              <q-icon name="description" />
            </q-item-section>

            <q-item-section> Ajax </q-item-section>
          </q-item>

          <q-item clickable v-ripple to="/map" exact>
            <q-item-section avatar>
              <q-icon name="shower" />
            </q-item-section>

            <q-item-section> Google Map </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import { date } from "quasar";

export default defineComponent({
  name: "MainLayout",

  setup() {
    const leftDrawerOpen = ref(false);

    return {
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
  computed: {
    today() {
      const timeStamp = Date.now();
      return date.formatDate(timeStamp, "D MMMM YYYY");
    },
  },
});
</script>

<style lang="scss">
.headerimg {
  height: 100%;
  opacity: 0.2;
}
</style>
