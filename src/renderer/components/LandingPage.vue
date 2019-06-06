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
        { rel: 'stylesheet', href: 'https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css'},
				{ rel: 'stylesheet', href: 'https://cdnjs.cloudflare.com/ajax/libs/jquery-datetimepicker/2.5.20/jquery.datetimepicker.min.css'}
			   ],
      script: [
        {type:'text/javascript', src: 'https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js', async:true, body: true, integrity:"sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T", crossorigin:"anonymous"},
        {type:'text/javascript', src: 'https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js'},
        {type:'text/javascript', src: 'https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.js'},
        {type:'text/javascript', src: 'https://unpkg.com/axios/dist/axios.min.js'},
				{type:'text/javascript', src: 'https://cdnjs.cloudflare.com/ajax/libs/jquery-datetimepicker/2.5.20/jquery.datetimepicker.full.min.js'}
      ]
    }
  }
</script>

<style>
.ild {
  background: #CAE9F5;
}



.buttonDownload {
	display: inline-block;
	position: relative;
	padding: 10px 25px;
  
	background-color: #4CC713;
	color: white;
  
	font-family: sans-serif;
	text-decoration: none;
	font-size: 0.9em;
	text-align: center;
	text-indent: 15px;
}

.buttonDownload:hover {
	background-color: #333;
	color: white;
}

.buttonDownload:before, .buttonDownload:after {
	content: ' ';
	display: block;
	position: absolute;
	left: 15px;
	top: 52%;
}

/* Download box shape  */
.buttonDownload:before {
	width: 10px;
	height: 2px;
	border-style: solid;
	border-width: 0 2px 2px;
}

/* Download arrow shape */
.buttonDownload:after {
	width: 0;
	height: 0;
	margin-left: 3px;
	margin-top: -7px;
  
	border-style: solid;
	border-width: 4px 4px 0 4px;
	border-color: transparent;
	border-top-color: inherit;
	
	animation: downloadArrow 2s linear infinite;
	animation-play-state: paused;
}

.buttonDownload:hover:before {
	border-color: #4CC713;
}

.buttonDownload:hover:after {
	border-top-color: #4CC713;
	animation-play-state: running;
}

/* keyframes for the download icon anim */
@keyframes downloadArrow {
	/* 0% and 0.001% keyframes used as a hackish way of having the button frozen on a nice looking frame by default */
	0% {
		margin-top: -7px;
		opacity: 1;
	}
	
	0.001% {
		margin-top: -15px;
		opacity: 0;
	}
	
	50% {
		opacity: 1;
	}
	
	100% {
		margin-top: 0;
		opacity: 0;
	}
}
</style>
