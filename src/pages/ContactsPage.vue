<template>
  <q-page>
    <div style="height: calc(100vh - 50px)">
      <!-- 🤿 Vue, please render the Google Map Component here -->
      <GMapMap :center="center" :zoom="10" />
    </div>
  </q-page>
</template>

<script setup>
const center = { lat: 51.093048, lng: 6.84212 };
</script>
