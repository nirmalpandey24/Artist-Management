<template lang="">
    <div class="mt-10 lg:w-[77vw] lg:h-72 md:h-58 md:w-[60vw] bg-white overflow-hidden rounded-lg w-[80vw] h-32">
        <img :src="`http://127.0.0.1:8000${userBanner}`" class="flex justify-center"/>
    </div>
</template>
<script setup>
import { ref, onMounted ,defineProps} from 'vue'
import axios from 'axios'

const props = defineProps({
  userBanner: {
    type:String,
    required:true
  }})

</script>
<style lang=""></style>
