<script setup>
  import Barber from './components/Barber.vue';
  import Appointment from './components/Appointment.vue';
</script>

<template>
  <div class="container-md mt-4">
    <div class="row">
      <div class="col-md my-4">
        <h2 class="mb-4">Fodrász hozzáadása</h2>
        <Barber/>
      </div>
      <div class="col-md my-4">
        <h2 class="mb-4">Foglalás</h2>
        <Appointment/>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
