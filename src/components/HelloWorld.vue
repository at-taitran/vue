<template>
  <div>
    <form>
      <div class="container">
        <h2>Select an image</h2>
        <input type="file" id="files" multiple="multiple" @change="onFileChange">
      </div>
      <div class="container wrap" v-if="images">
        <div class="b-img" v-for="(i, index) in images" :key="index">
          <img :src="i"/>
          <button @click="removeImage(index)">Remove image</button>
        </div>
      </div>
      <div class="container">
        <button v-on:click="submitFile()" type="submit">Upload</button>
      </div>
    </form>  
  </div>
</template> 
<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios, axios)
export default {
  name: 'HelloWorld',
  data () {
    return {
      imageData: null,
      images: [],
      currentFile:[undefined]
    }
  },
  methods: {
    onFileChange(e) {
      const files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.createImage(files);
    },
    createImage(files) {
      const vm = this;
      for (var index = 0; index < files.length; index++) {
        let reader = new FileReader();
        reader.onload = function(event) {
          const imageUrl = event.target.result;
           vm.images.push(imageUrl);
        } 
        reader.readAsDataURL(files[index]);
      }
    },
    removeImage(index) {
      this.images.splice(index.splice(index), 1)
      console.log(this.images[index ])
    },
    // submitFile(e) {
    //   let formData = new FormData();
    //   let v = this;
    //   const files = e.target.files || e.dataTransfer.files; 
    //   for (let index = 0; index < files.length; index++) {
    //     let file = v.files[index];
    //     formData.append('files[' + index + ']', file);
    //   }
    //   axios.post('/select-files',
    //   formData,{
    //     header:{
    //       'Content-Type': 'multipart/form-data'
    //     }
    //   }
    //   ).then(function() {
    //     console.log('SUCCESS!!');
    //   }).catch(function() {
    //     console.log('FAIL!!');
    //     console.log(formData)
    //   })
    //   // ,
    //   // this.handleFileUpload();
    // },
    // handleFileUpload() {
    //   let uploadFiles = this.$refs.files.files;
    //   for (let index = 0; index < uploadFiles.length; index++) {
    //     this.files.push(uploadFiles[index]);
    //   }
    //   console.log(uploadFiles)
    // }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container{
  width: 1200px;
  margin-left: auto;
  margin-right:auto;
  padding: 15px
}
.show{
  width: auto;
  height: 100px;
}
img {
  width: 200px;
  height: 75px;
  margin: auto;
  display: block;
  margin-bottom: 10px;
  object-fit: cover;
}
.b-img{
  padding: 1rem;
}
.wrap{
  width: 100%;
  max-width: 1200px;
  margin-top: 20px;
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
}
</style>
