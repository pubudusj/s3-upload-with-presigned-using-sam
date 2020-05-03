<template>
  <input type="file" id="file" ref="file" v-on:change="handleFileUpload()" />
</template>

<script>
import axios from 'axios'

export default {
  name: 'FileUpload',
  data () {
    return {
      file: '',
      generateUrlEndpoint: process.env.VUE_APP_GENERATE_URL
    }
  },
  methods: {
    handleFileUpload () {
      this.file = this.$refs.file.files[0]
      this.generateUploadUrl()
    },
    submitFile (targetUrl) {
      var formData = new FormData()
      formData.append('file', this.file)
      axios
        .put(targetUrl, formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        .then(function () {
          console.log('SUCCESS!!')
        })
        .catch(function () {
          console.log('FAILURE!!')
        })
    },
    generateUploadUrl () {
      var self = this
      axios
        .get(this.generateUrlEndpoint)
        .then(function (response) {
          self.submitFile(response.data)
        })
        .catch(function (e) {
          console.log(e)
          console.log('Fail to generate url')
        })
    }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
