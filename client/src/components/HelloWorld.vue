<template>
  <div class="hello">
    <vue-dropzone ref="myVueDropzone" id="dropzone" :options="dropzoneOptions"></vue-dropzone>
  </div>
</template>

<script>
import vue2Dropzone from 'vue2-dropzone'
import axios from 'axios'
import 'vue2-dropzone/dist/vue2Dropzone.min.css'

export default {
  name: 'HelloWorld',
  data: function () {
    return {
      dropzoneOptions: {
        url: `http://localhost:8888/images`,
        method: 'post',
        addRemoveLinks: 'true'
      }
    }
  },
  mounted () {
    axios.get('http://localhost:8888/images').then(res => {
      res.data.forEach(res => {

        let filename = res.path.replace('http://localhost:8888/', '')
        let id = filename.replace('.png', '')
        var file = {size: res.size, name: filename, type: "image/png", upload: {uuid: id}}
        this.$refs.myVueDropzone.manuallyAddFile(file, res.path)
      })
    }).catch(err => {
      console.error(err)
    })
  },
  components: {
    vueDropzone: vue2Dropzone
  },
  methods: {
    sendingEvent: function (file, xhr, formData) {
      formData.append('uuid', file.upload.uuid)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
