<template>
  <q-page>
    <GMapMap
      :center="center"
      :zoom="12"
      map-type-id="terrain"
      style="height: calc(100vh - 50px)"
    >
      <GMapCluster :zoomOnClick="true">
        <GMapMarker
          :key="index"
          v-for="(m, index) in markers"
          :position="m.position"
          :clickable="true"
          :draggable="true"
          @click="center = m.position"
        />
      </GMapCluster>
    </GMapMap>
  </q-page>
</template>

<script setup>
const center = { lat: 45.470014, lng: 9.183974 };
const markers = [
  {
    position: {
      lat: 45.49972,
      lng: 9.208701,
    },
  },
  {
    position: {
      lat: 45.487205,
      lng: 9.14965,
    },
  },
  {
    position: {
      lat: 45.470595,
      lng: 9.156345,
    },
  },
  {
    position: {
      lat: 45.407962,
      lng: 9.171622,
    },
  },
];
</script>
