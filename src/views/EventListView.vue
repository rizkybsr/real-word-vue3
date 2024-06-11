<script setup>
  import EventCard from '@/components/EventCard.vue';
  import { ref, onMounted} from 'vue'
  import EventService from '@/services/EventService';

  const events = ref(null)

  // This is called a lifecycle hook, this is called when the component is mounted 
  // There are heaps of Lifecycle hooks for example onMounted, onUpdated, onBeforeMount etc
  onMounted(() => {
    // Axios API call get request
    EventService.getEvents()
    .then((response) => {
      events.value = response.data
    })
    .catch((error) => {
      console.log(error)
    })
  })

</script>

<template>
  <main>
    <h1>Events For Good</h1>
    <div class="events">
      <EventCard v-for="(event, index) in events" :key="event.id" :event="event"></EventCard>
    </div>
  </main>
</template>

<style scoped>
  .events {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

</style>
