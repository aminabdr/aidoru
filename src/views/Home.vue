

<template>
  <div class="bg-gray-50 px-8">
    <p>{{ error }}</p>
    <p>{{ decodedString }}</p>
    <button @click="torch=!torch">TURN ON/OFF FLASHLIGHT</button>
    <qrcode-stream @init="onInit" @decode="onDecode" :torch="torch"></qrcode-stream>
  </div>
</template>

<script>

import { QrcodeStream } from 'vue3-qrcode-reader'
export default {
  data() {
    return {
      error: '',
      decodedString: '',
      torch: false,
    }
  },

  components: {
    QrcodeStream
  },
  methods: {
    async onInit( promise ) {
      try {
      const { capabilities } = await promise

      // successfully initialized
    } catch (error) {
      if (error.name === 'NotAllowedError') {
        this.error = "user denied camera access permisson"
      } else if (error.name === 'NotFoundError') {
        this.error = "no suitable camera device installed"
      } else if (error.name === 'NotSupportedError') {
        this.error = "page is not served over HTTPS (or localhost)"
      } else if (error.name === 'NotReadableError') {
        this.error = "maybe camera is already in use"
      } else if (error.name === 'OverconstrainedError') {
        this.error = " did you requested the front camera although there is none?"
      } else if (error.name === 'StreamApiNotSupportedError') {
        this.error = "browser seems to be lacking features"
      }
      } finally {
        // hide loading indicator
      }
    },
    onDecode(decodedString) {
      this.decodedString = decodedString;
      //window.location.replace(decodedString)
    }
  }
}
</script>
