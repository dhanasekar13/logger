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
        { rel: 'stylesheet', href: 'https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css'}
      ],
      script: [
        {type:'text/javascript', src: 'https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js', async: true, body: true},
        {type:'text/javascript', src: 'https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js', async:true, body: true, integrity:"sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T", crossorigin:"anonymous"},
        {type:'text/javascript', src: 'https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js'},
        {type:'text/javascript', src: 'https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.js'},
        {type:'text/javascript', src: 'https://unpkg.com/axios/dist/axios.min.js'}
      ]
    }
  }
</script>

<style>
.ild {
  background: #CAE9F5;
}


</style>
