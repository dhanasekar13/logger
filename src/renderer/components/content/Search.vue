<template>
    <div>
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
  <div v-if="add != 0">
        {{add}} <a href="#" v-on:click="remove()">X</a>
  </div>
  </div>
</template>

<script>
 export default {
    name: 'Search',
    props:['content'],
    data() {
        return {
          whole: '',
          date1: '',
          date2: '',
          text1: '',
          count: 0,
          searchcount: 0,
          newsearch : '',
          searchindex: 0,
          add: 0,
          newcontent: ''
        }
    },
    methods: {
       counted: function (data){
          var self = this
          window.find(self.temp)
          console.log('------',self.searchindex)
          self.searchindex = self.content.indexOf(self.temp, self.searchindex + 1)
          let start = self.searchindex
          console.log('hitted--------',self.searchindex)
          self.newsearch = self.content.substring(start, 300)
      },
       hit: function (event){
         var self = this
         console.log('--------Prop data----------',self.content,'-------------------')
         
          event.preventDefault(); 
          let replace = new RegExp(self.text1, "g")
          self.count =  self.content.match(replace).length
          self.temp = self.text1
          self.text1 = ''
          self.searchindex = self.content.indexOf(self.temp, self.searchindex + 1)
          console.log(self.searchindex,'-------------')
          self.newsearch = self.content.substring(self.searchindex, 500)
      },
      change: function(){
        var self = this
        self.whole = self.content
        let current_datetime = new Date(self.date1)
        self.startd= current_datetime.toString().substring(0, 15);
        let current_datetime1 = new Date(self.date2)
        self.endd = current_datetime1.toString().substring(0, 15);
        console.log(self.startd,'--------------',self.endd)
        let start = self.content.indexOf(self.startd)
        let end = self.content.lastIndexOf(self.endd)
        console.log(start,'--------time--',end)
        self.newcontent = self.content.substring(start, end)
        this.$emit('changecontent', self.newcontent)
        this.add = 'last filter'
        this.date1 = ''
        this.date2 = ''
        },
        remove: function (){
          this.add =0
          this.$emit('changecontent',this.whole)
            this.date1 = ''
            this.date2 = ''
        }
    }
 }
</script>

<style>

</style>