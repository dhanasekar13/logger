<template>
  <div id="wrapper">
    <div class="row">
        <div class="col-sm-3 col-md-3 ild">
        <Sidebar />
        </div>
        <div class="col-sm-3 col-md-9">
        <Content />
        </div>
    </div>
  </div>
</template>

<script>
 import Sidebar from './Sidebar'
 import Content from './Content'

  export default {
    name: 'landing-page',
    components: { 
      Sidebar, Content
       },
    methods: {
     
    },
    head:{
      title:{
        inner: 'Search Logger'
      },
      link:[
        { rel: 'stylesheet', href: 'https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css'}
      ],
      script: [
        {type:'text/javascript', src: 'https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js', async: true, body: true},
        {type:'text/javascript', src: 'https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.bundle.min.js', async:true, body: true}
 
      ]
    }
  }
</script>

<style>
.ild {
  background: #CAE9F5;
}


</style>
