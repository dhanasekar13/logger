<template>
    <div class="container">
  <div class="row">
   <div class="col-sm">
   
    <label>From date :</label> <br />
      <input type="datetime-local" id="date1" class="form-control" v-model="date1">
    </div>
    <div class="col-sm">
       <label>To date : </label><br />
      <input type="datetime-local" id="date2" class="form-control" v-model="date2" v-on:change="change()"/>
    </div>
     <div class="col-sm">
     <label>Search :</label><br />
   
          <form v-on:submit="hit" class="form-inline my-2 my-lg-0">
            <input class="form-control mr-sm-2" type="search"  placeholder="Search" aria-label="Search" v-model="text1" >
          </form>
    </div>
    <div class="col-sm" v-if="count>0">
<span>{{temp}} <a href="#" v-on:click="countedb(searchcount--)"><< </a> </span><input type="number" class="count" v-model="searchcount" v-on:change="countedf(searchcount)" min=0 v-bind:max="count"/> <a href="#" v-on:click="countedf(searchcount++)"> >> </a><span> ({{count}}) </span>
    </div>
    <span style="color:red" v-else>{{msg}}</span>
  </div>
  </div>
</template>

<script>
import $ from 'jquery'
import { serverBus } from '../../main'
 const fs = require('fs')
const {dialog} = require('electron').remote
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
          newcontent: '',
          msg:''
        }
    },
     components: { 
    
       },
        
        created() {
      var self = this
       $("#date1").datetimepicker('show')
      serverBus.$on('searchtext', (calltext)=>{
         let replace = new RegExp(calltext, "g")
          self.count =  self.content.match(replace).length
          self.searchcount = 0
          self.temp = calltext
          self.text1 = ''
          self.newsearch = self.content.substring(0, 300)
          self.searchindex = self.content.indexOf(self.temp, 0)
        return 1;
      })
      serverBus.$on('searchdat', (calldat)=>{
         let current_datetime = new Date(calldat.sdate)
        self.startd= current_datetime.toString().substring(0, 15);
        let current_datetime1 = new Date(calldat.edate)
        self.endd = current_datetime1.toString().substring(0, 15);
        console.log(self.startd,'--------------',self.endd)
        let start = self.content.indexOf(self.startd)
        let end = self.content.lastIndexOf(self.endd)
        console.log(start,'--------time--',end)
        self.newcontent = self.content.substring(start, end)
        this.$emit('changecontent', self.newcontent)
        this.date1 = ''
        this.date2 = ''
      })
      serverBus.$on('downloaddat', (calldat)=>{
         let current_datetime = new Date(calldat.sdate)
        self.startd= current_datetime.toString().substring(0, 15);
        let current_datetime1 = new Date(calldat.edate)
        self.endd = current_datetime1.toString().substring(0, 15);
        console.log(self.startd,'--------------',self.endd)
        let start = self.content.indexOf(self.startd)
        let end = self.content.lastIndexOf(self.endd)
        console.log(start,'--------time--',end)
        self.newcontent = self.content.substring(start, end)
        dialog.showSaveDialog((fileName) => {
             if (fileName === undefined){
                 console.log("You didn't save the file");
                 return;
              }
             fs.writeFile(fileName, self.newcontent, (err) => {
                if(err){
                   alert("An error ocurred creating the file "+ err.message)
                }
                    
        alert("The file has been succesfully saved");
           });
        }); 
      })
    },
    methods: {
       countedf: function (data){
          var self = this
          window.find(self.temp)
          self.newsearch = self.content.substring(self.searchindex, 300)
          self.searchindex = self.content.indexOf(self.temp, self.searchindex + 1)
          this.$emit('changetext', self.newsearch)
      },
       countedb: function (data){
          var self = this
          window.find(self.temp,0,1)
          self.newsearch = self.content.substring(self.searchindex, 300)
          self.searchindex = self.content.indexOf(self.temp, self.searchindex + 1)
          this.$emit('changetext', self.newsearch)
      },
       hit: function (event){
         var self = this
         self.msg =''
          event.preventDefault(); 
          let replace = new RegExp(self.text1, "g")
          console.log(self.content.match(replace),'------------replace')
          if(self.content.match(replace) != null){
              self.count =  self.content.match(replace).length
          }else {
            self.count = 0
            self.msg = 'NO search result is found'
          }
          self.searchcount = 0
          self.temp = self.text1
          self.text1 = ''
           window.find(self.temp,0,0,1)
          self.newsearch = self.content.substring(self.searchindex, 300)
          self.searchindex = self.content.indexOf(self.temp, self.searchindex + 1)
          let datasend = {
            text: self.temp
          }
          serverBus.$emit('search', datasend)
          this.$emit('changetext', self.newsearch)
      },
      change: function(){
        var self = this
        self.whole = self.content
        let current_datetime = new Date(self.date1)
        self.startd= current_datetime.toString().substring(0, 15);
        let current_datetime1 = new Date(self.date2)
        self.endd = current_datetime1.toString().substring(0, 15);
        let start11 = current_datetime.toString().substring(25,15)
        
        let start12 = current_datetime1.toString().substring(25,15)
        console.log(start11,'--------------dhansekar-------',start12)

        console.log(self.startd,'--------------',self.endd)
        let start = self.content.indexOf(self.startd)
        let end = self.content.lastIndexOf(self.endd)
        console.log(start,'--------time--',end)
        self.newcontent = self.content.substring(start, end)
        this.$emit('changecontent', self.newcontent)
        this.add = 'last date filter ('+start+'---'+end+')'
        this.date1 = ''
        this.date2 = ''
        let dat1 = {
          sdate: self.startd,
          edate: self.endd
        }
        serverBus.$emit('datesearch', dat1)
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