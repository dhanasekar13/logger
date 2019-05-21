<template >
<div class="full">
  <div >
    <h3>Search for File..</h3>
  </div>
 <div class="md-form mt-0 file1">
  <tr v-for="(item, sear) in search">
    <td><a href="#" v-on:click="call(item.dir)">{{item.name}}</a></td>

  </tr>
</div>
</div>
</template>

<script>
  import { serverBus } from '../main'
  const fs = require('fs')
  const path = require('path');
  const {dialog} = require('electron').remote
  export default {
    name: 'Sidebar',
    data() {
        return {
            search:[],
            text1: '',
            text2: ''
        }
    },
      created() {
      var self = this
      serverBus.$on('uploadFileContent1', (server)=>{
         console.log('even it came here too',server.name)
         let filename = path.parse(server.name).name;
         self.search.push({dir:server.name, name: filename})
         console.log(self.search)
         return 1;
      })
    },
    components: { 
      
       },
    methods: {
      call:function(filename){
         fs.readFile(filename,'utf-8', function(err, data){
                 if(err){
                     return 1;
                 }
                 
                  self.result ={
                     data,
                     name:filename
                 } 
                  serverBus.$emit('selectedFileContent', self.result);
                
             })

      }
    }
  }
</script>

<style>

.full{
  height:100%;
  min-height:96%;
      height: 100vh;
}
.file1{

  margin-left: 20px;
}
</style>
