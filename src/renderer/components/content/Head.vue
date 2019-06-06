<template>
  <div>
  <nav id="navbar-example2" class="navbar navbar-light bg-light ">
  <a class="navbar-brand " href="#">Logger</a>
  <ul class="nav nav-pills">
   <li class="nav-item dropdown" style="width:450px;" >
    <form v-on:submit="hit" class="form-inline my-2 my-lg-0">
            <input style="width:450px;" type="text" placeholder="place the url" v-model="url"/>
            </form>
    </li>
    <li class="nav-item dropdown">
     <button type="button" v-on:click="upload" class="btn btn-dark">Upload Log File</button>
    </li>
  </ul>
</nav>
  </div>
</template>

<script>
import { serverBus } from '../../main'
const http = require("http");
 const crypto = require('crypto');
 const fs = require('fs')
 const request = require('request')
 const {dialog} = require('electron').remote
  const getStream = require('get-stream');
      
  export default {
    name: 'Head',
    data() {
        return {
            result: {},
            filename: '',
            url: ''
        }
    },
    components: { 
      
       },
    methods: {
      hit: function(event) {
        let fs1 = require('fs')
        console.log('came here')
        var self = this
        event.preventDefault(); 
        axios.get(self.url,{
          headers: {
        	  crossdomain: true
	        },
        	proxy: {
	           host: '169.61.207.8',
	           port: 3032
	        }
        })
  .then(function (response) {
    console.log(response);
      let result1 = {
                    name:response.request.responseURL
                  }
                     serverBus.$emit('uploadFileContent1', result1);
                 self.result ={
                     data: ''+response.data+ '',
                     name:response.request.responseURL || 'called url'
                 } 

                 console.log('INside upload server trigger')
                  serverBus.$emit('uploadFileContent', self.result);
                return 1
  })
  .catch(function (error) {
    console.log(error);
  });
      },
     upload: async function(event) {
         var self = this
         dialog.showOpenDialog((fileNames) =>{
             if(fileNames === undefined){
                 return 1;
             }
             console.log('--------- This is filename----------',fileNames[0])
             self.filename = fileNames[0]
             /*  fs.readFile(fileNames[0],'utf-8', function(err, data1){
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
             }) */        
             let alldata =''
             let readStream = fs.createReadStream(fileNames[0]);
                let result1 = {
                    name:self.filename || fileNames[0]
                }
                readStream.on('data',(buff)=> alldata += buff.toString())
                .on('end',()=> {
                       self.result ={
                     data:alldata,
                     name:self.filename || fileNames[0]
                } 
                serverBus.$emit('uploadFileContent1', result1);
                serverBus.$emit('uploadFileContent', self.result);
               })
         })
     }
    }
  }
</script>

<style>

</style>
