<script setup>
import { ref, onMounted } from 'vue'
import EventService from '@/services/EventService';

const event = ref(null)

// ID is passed in from EventCard.vue as a prop.
const props = defineProps({
  id: {
    required: true
  }
})

onMounted(() => {
    EventService.getEvent(props.id)
        .then((response) => {
            event.value = response.data
        })
        .catch((error) => {
            console.log(error)
        })
})

</script>

<template>
    <!-- v-if is used to wait for the API call to finish and then display the data -->
    <div v-if="event">
        <h1>{{ event.title }}</h1>
        <p>{{ event.time }} on {{ event.date }} @ {{ event.location }}</p>
        <p>{{ event.description }}</p>
    </div>
</template>