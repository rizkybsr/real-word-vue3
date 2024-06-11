<script setup>
  import EventCard from '@/components/EventCard.vue';
  import { ref, onMounted, computed, watchEffect } from 'vue'
  import EventService from '@/services/EventService';

  const props = defineProps(['page'])

  const events = ref(null)
  const totalEvents = ref(0)

  const hasNextPage = computed(() => {
    const totalPages = Math.ceil(totalEvents.value / 2)
    return page.value < totalPages
  })

  const totalPages = computed(() => {
    return Math.ceil(totalEvents.value / 2)
  })

  const page = computed(() => props.page)

  // This is called a lifecycle hook, this is called when the component is mounted 
  // There are heaps of Lifecycle hooks for example onMounted, onUpdated, onBeforeMount etc
  onMounted(() => {

    watchEffect(() => {
      events.value = null
      // Axios API call get request
      EventService.getEvents(2, page.value)
      .then((response) => {
        events.value = response.data
        totalEvents.value = response.headers['x-total-count']
      })
      .catch((error) => {
        console.log(error)
      })
    }) 
  })

</script>

<template>
  <main>
    <h1>Events For Good</h1>
    <div class="events">
      <EventCard v-for="(event, index) in events" :key="event.id" :event="event"></EventCard>

      <div class="pagination">
        <router-link 
        id="page-prev"
        :to="{ name: 'event-list', 'query': { page: page - 1} }" 
        rel="prev"
        v-if="page != 1"
        >&#60; Prev</router-link>

        <router-link
        v-for=" pageNum in totalPages"
        :to="{ name: 'event-list', 'query': { page: pageNum} }"
        rel="pageNum" 
        >{{ pageNum }}</router-link>
        
        <router-link 
        id="page-next"
        :to="{ name: 'event-list', 'query': { page: page + 1} }" 
        rel="prev"
        v-if="hasNextPage"
        >&#62; Next</router-link>
      </div>
    </div>
      
  </main>
</template>

<style scoped>
  .events {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .pagination {
    display: flex;
    width: 290px;
  }

  .pagination a {
    flex: 1;
    text-decoration: none;
  }

  #page-next {
    text-align: right;
  }

  #page-prev {
    text-align: left;
  }

</style>
