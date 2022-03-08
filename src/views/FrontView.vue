<template>
<FrontBar></FrontBar>
<router-view></router-view>
</template>>

<script>
import FrontBar from '@/components/FrontBar.vue'
export default {
  components: {
    FrontBar
  }
}
</script>
