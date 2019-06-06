<template >
<div class="full">
  <div >
    <h3>Search for File..</h3> 
  </div>
 <div class="md-form mt-0 file1">
 <div align="right" v-if="show > 0">
 <button  class="btn btn-warning btn-sm" v-on:click="removeall()">Clear All Search</button>
 </div>
  <tr v-for="(item, sear) in search">
    <td><a href="#" v-on:click="call(item.dir, sear)">{{item.name}}</a>
      <ul style="list-style: none">
      <li  v-for="(item1, index) in item.sub">
        <a class="link" v-on:click="searchtext(item1)" href="#">{{item1}}</a> <a href="#" v-on:click="remove(index, sear)"><pre> X  </pre></a>
        </li>
         <li class="link1" v-for="(item2, index1) in item.dat">
        <a class="link" v-on:click="searchdat(item2)" href="#">{{item2.sdate}} - to - {{item2.edate}}</a> <a href="#" v-on:click="remove1(index1, sear,item.dir)"><pre> X  </pre></a>
       <a href="#"><span v-on:click="downloaddat(item2)" >||</span></a>
        </li>
      </ul>
    </td>

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
            text2: '',
            count: 0,
            show:0
        }
    },
      created() {
      var self = this
      serverBus.$on('uploadFileContent1', (server)=>{
         console.log('even it came here too',server.name)
         let filename = path.parse(server.name).name;
         self.count = self.search.push({dir:server.name, name: filename, sub:[], dat:[]})
         return 1;
      })
      serverBus.$on('search', (search)=>{
        console.log('the current searc file is ' , this.count)
        self.search[this.count - 1].sub.push(search.text)
        self.show++
        return 1;
      })
      serverBus.$on('datesearch', (data1)=>{
        console.log('----------date search=====',data1)
        self.search[this.count - 1].dat.push(data1)
        self.show++
        return 1;
      })
    },
    components: { 
      
       },
    methods: {
      call:function(filename, sear){
        var self = this
                 self.count = sear + 1
                 console.log('the current file count',self.count)
                 serverBus.$emit('selectedFileContent', sear);
      },
      remove:function(value, fileindex){
        this.show--
        this.search[fileindex].sub.splice(value,1);
      },
      remove1:function(value, fileindex, filename){
        this.show--
        this.search[fileindex].dat.splice(value,1);
        this.call(filename, fileindex)
      },
      removeall:function(){
       let search1 = this.search.map(data => ({dir:data.dir, name: data.name, sub:[]}))
       this.search = search1
       this.show = 0
      },
      searchtext: function(text) {
           serverBus.$emit('searchtext', text)
      },
      searchdat: function(dat1){
        serverBus.$emit('searchdat', dat1)
      },
      downloaddat: function (dat1){
        serverBus.$emit('downloaddat', dat1)
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
.link {
    color: #ff0000;
    cursor: pointer;
    display: inline-block;
}
.link:hover  { 
    color: #cc99ff;
}
pre:{
  display:inline
}
td ul a {
    display: inline-flex;
}
td ul a :hover {
    color: red;
}
ul .link1 {
  display: inline-flex;
}


</style>
