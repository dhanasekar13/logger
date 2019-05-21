<template>
  <div>
  <nav class="navbar navbar-expand-sm bg-light">

  <ul class="navbar-nav">
    <li class="nav-item">From date : 
      <input type="date" class="form-control" v-model="date1"/>
    </li>
    <li class="nav-item">To date : 
      <input type="date" class="form-control" v-model="date2" v-on:change="change()"/>
    </li>
    <li class="nav-item">Search :
          <form v-on:submit="hit">
            <input class="form-control" type="text" placeholder="Search" aria-label="Search" v-model="text1">
          </form>
    </li>
    <li class="nav-item" v-if="count>0">
<span>{{temp}} <a href="#" v-on:click="counted(searchcount--)"><< </a> </span><input type="text" class="count" v-model="searchcount" v-on:change="counted(searchcount)"/> <a href="#" v-on:click="counted(searchcount++)"> >> </a><span> ({{count}}) </span>
    </li>
  </ul>
 

</nav>
  <div id="mini">
     <pre>{{mini}}</pre>
  </div>
  <textarea class="text1">{{newsearch}}</textarea>
  </div>
</template>

<script>
 import { serverBus } from '../../main'
 import Search from './Search'
  export default {
    name: 'MiniContent',
    data(){
      return {
        mini: '',
        date1: '',
        date2: '',
        text1: '',
        temp: '',
        startd: '',
        endd:'',
        count: 0,
        searchcount: 0,
        newsearch : '',
        searchindex: 0
      }
    },
    created() {
      var self = this
      serverBus.$on('uploadFileContent', (server)=>{
        console.log('came before correct',server.data)
        self.mini = server.data
      })
      serverBus.$on('selectedFileContent', (server)=>{
        self.mini = server.data
        self.count = 0
        self.temp = ''
      })
    },
    components: { 
      Search
       },
    methods: {
      counted: function (data){
          var self = this
          window.find(self.temp)
          console.log('------',self.searchindex)
          self.searchindex = self.mini.indexOf(self.temp, self.searchindex + 1)
          let start = self.searchindex
          console.log('hitted--------',self.searchindex)
          self.newsearch = self.mini.substring(start, 300)
      },  
       hit: function (event){
         var self = this
          event.preventDefault(); 
          let replace = new RegExp(self.text1, "g")
          self.count =  self.mini.match(replace).length
          self.temp = self.text1
          self.text1 = ''
          self.searchindex = self.mini.indexOf(self.temp, self.searchindex + 1)
          console.log(self.searchindex,'-------------')
          self.newsearch = self.mini.substring(self.searchindex, 500)
      },
      change: function(){
        var self = this
        let current_datetime = new Date(self.date1)
        self.startd= current_datetime.toString().substring(0, 15);
        let current_datetime1 = new Date(self.date2)
        self.endd = current_datetime1.toString().substring(0, 15);
        console.log(self.startd,'--------------',self.endd)
        let start = self.mini.indexOf(self.startd)
        let end = self.mini.indexOf(self.endd)
        console.log(start,'--------time--',end)
        self.mini = self.mini.substring(start, end)
        }
    }
  }
</script>

<style>
#mini{
      height: 400px;
    overflow: scroll;
    overflow-x: hidden;
}
.text1{
  width:98%;
  min-height:100px;
}
.nav-item{
      margin-right: 10px;
    margin-left: 10px;
}
.count{
      width: 16px;
}
</style>
