<template>
  <div class="root">
    <UploadComponent />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import UploadComponent from '@/components/UploadComponent.vue'

export default defineComponent({
  components: { UploadComponent },
  data () {
    return {
      file: null as File | null
    }
  }
})
</script>
