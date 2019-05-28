<template>
  <div>
  <nav class="navbar navbar-expand-sm bg-light">
  <Search :content="mini" v-on:changecontent="onChildChange" v-on:changetext="changetextChange" ></Search>
 
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
 import Result from './Result'

  export default {
    name: 'MiniContent',
    data(){
      return {
        entireObj: [],
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
        self.entireObj.push(server.data)
        self.mini = server.data
        self.newsearch= ''
      })
      serverBus.$on('selectedFileContent', (server)=>{
        self.mini = self.entireObj[server]
        self.count = 0
        self.temp = ''
        self.newsearch= ''
      })
    },
    components: { 
      Search, Result
       },
    methods: {
        onChildChange: function(val){
          var self = this
          self.mini = val
          self.newsearch = ''
        },
        changetextChange: function(val){
          var self = this 
          console.log(val,'----in minicontent---NEW SEARCH-------------',val)
          self.newsearch = val
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

.nav-item{
      margin-right: 10px;
    margin-left: 10px;
}
.count{
      width: 30px;
}
</style>
