<template>
  <div
    class="group ml-2 lg:ml-5 w-40 h-54 border-2 border-transparent hover:bg-secondary-color rounded-2xl flex flex-col justify-start items-start p-3 gap-1 hover:transition-all cursor-pointer hover:shadow-[0_0_2px_#fff,inset_0_0_2px_#FF516D,0_0_4px_#FF516D,0_0_15px_#FF516D] hover:border-2 hover:border-white text-primary-text-color hover:text-dark-primary-color group"
    :style="{
      color: `${user?.theme?.secondaryColor}`
    }"
  >
    <RouterLink :to="'/music/' + musicData.id">
      <img :src="`http://127.0.0.1:8000${musicData.img_profile}`" class="w-32 h-32 rounded-lg" />
    </RouterLink>
    <div class="flex justify-between w-full items-center mt-2">
      <div class="flex flex-col">
        <RouterLink
          :to="'/music/' + musicData.id"
          class="text-md font-semibold text-center hover:underline"
        >
          {{ musicData.name }}
        </RouterLink>
        <RouterLink :to="'/artist/' + musicData.artist" class="text-sm opacity-50 hover:underline">
          {{ musicData.artist_name }}
        </RouterLink>
        <RouterLink :to="'/album/' + musicData.album" class="text-sm opacity-50 hover:underline">
          {{ musicData.album_name }}
        </RouterLink>
      </div>
      <!-- <v-icon
        name="md-playcircleoutline-round"
        @click="playMusic"
        fill="#302f31"
        scale="1.5"
        class="cursor-pointer"
      /> -->
    </div>
  </div>
</template>

<script>
export default {
  props: {
    musicData: {
      type: Object,
      required: true
    },
    user: {}
  },

  methods: {
    playMusic() {
      this.$store.dispatch('setMusicPlayer', this.musicData)
      console.log(this.musicData)
    }
  }
}
</script>

<style scoped>
.group:hover {
  color: white !important;
}
</style>
