<template>
  <PageLayoutWithPlayer id="display-flex">
    <template #content>
    <div class="flex gap-4 flex-wrap flex-grow justify-center ">
          <!-- <BandCard v-for="x in bandData" class="p-5" :bandDetail="x" linkto="band"/> -->
    </div>
    </template>
  </PageLayoutWithPlayer>
</template>

<script>
// import BandCard from '@/components/cards/BandCard.vue'
import { mapState } from 'vuex';
export default{
    data(){
      return {
         bandData :[
  { id: 1, name: 'Album 1', artist: 'Artist 1' },
  { id: 2, name: 'Album 2', artist: 'Artist 2' },
  { id: 3, name: 'Album 3', artist: 'Artist 3' },
  { id: 4, name: 'Album 4', artist: 'Artist 4' },
  { id: 5, name: 'Album 5', artist: 'Artist 5' },
  { id: 6, name: 'Album 6', artist: 'Artist 6' }
]
      }
    }
}

</script>