<template>
  <div class="home">
    <Stockinfo/>
  </div>
</template>

<script>
// @ is an alias to /src
import Stockinfo from '@/components/Stockinfo.vue'

export default {
  name: 'Home',
  components: {
    Stockinfo
  }
}
</script>
