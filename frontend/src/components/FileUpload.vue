<template>
  <v-app id="inspire">
    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-row
          align="center"
          justify="center"
        >
          <v-col
            cols="12"
            sm="8"
            md="4"
          >
          <v-alert v-if="uploadStatus == 1" type="success">
            File uploaded successfully.
          </v-alert>
          <v-alert v-if="uploadStatus == 0" type="error">
            Failed uploading the image. Please try again.
          </v-alert>
            <v-card class="elevation-12">
              <v-toolbar
                color="primary"
                dark
                flat
              >
                <v-toolbar-title>Upload File</v-toolbar-title>
              </v-toolbar>
              <v-card-text>
                <v-form ref="form">
                  <v-file-input
                    label="Select file to upload"
                    v-model="uploadedFile"
                    append-icon
                  />
                  <v-progress-linear
                    color="primary accent-4"
                    indeterminate
                    rounded
                    height="4"
                    v-if="loading"
                  />
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer />
                <v-btn color="primary" :disabled="disabled" v-on:click="handleFileUpload ()">Upload</v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import axios from 'axios'

export default {
  name: 'FileUpload',
  data () {
    return {
      uploadedFile: null,
      generateUrlEndpoint: process.env.VUE_APP_GENERATE_URL,
      loading: false,
      uploadStatus: null
    }
  },
  computed: {
    disabled () {
      return !this.uploadedFile
    }
  },
  methods: {
    handleFileUpload () {
      if (this.uploadedFile) {
        this.loading = true
        this.generateUploadUrl()
      } else {
        console.log('No files found')
      }
    },
    submitFile (targetUrl) {
      var formData = new FormData()
      var self = this
      formData.append('file', this.uploadedFile)
      axios
        .put(targetUrl, formData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        })
        .then(function () {
          self.resetForm()
          self.loading = false
          self.uploadStatus = 1
        })
        .catch(function () {
          self.resetForm()
          self.loading = false
          self.uploadStatus = 0
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
          self.loading = false
          self.uploadStatus = 0
        })
    },
    resetForm () {
      this.$refs.form.reset()
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
