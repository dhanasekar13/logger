<template>
  <div>
  <nav id="navbar-example2" class="navbar navbar-light bg-light">
  <a class="navbar-brand" href="#">Logger</a>
  <ul class="nav nav-pills">
    <li class="nav-item dropdown">
     <button type="button" v-on:click="upload" class="btn btn-dark">Upload Log File</button>
    </li>
  </ul>
</nav>
  </div>
</template>

<script>
import { serverBus } from '../../main'
 const fs = require('fs')
 const {dialog} = require('electron').remote
  export default {
    name: 'Head',
    data() {
        return {
            result: {},
            filename: ''
        }
    },
    components: { 
      
       },
    methods: {
     upload: function (event) {
         var self = this
         dialog.showOpenDialog((fileNames) =>{
             if(fileNames === undefined){
                 return 1;
             }
             console.log('--------- This is filename----------',fileNames[0])
             self.filename = fileNames[0]
             fs.readFile(fileNames[0],'utf-8', function(err, data1){
                 if(err){
                     return 1;
                 }
                   let result1 = {
                    name:self.filename || fileNames[0]
                  }
                     serverBus.$emit('uploadFileContent1', result1);
                 self.result ={
                     data: ''+data1+ '',
                     name:self.filename || fileNames[0]
                 } 

                 console.log('INside upload server trigger')
                  serverBus.$emit('uploadFileContent', self.result);
                return 1
             })
         })
     }
    }
  }
</script>

<style>

</style>
