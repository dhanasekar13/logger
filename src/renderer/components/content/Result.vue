<template>
  <textarea class="text1">{{value1}}</textarea>
</template>

<script>

export default {
    name: 'Result',
     props:['val'],
    data(){
        return {
            value1:this.val
        }
    }
}
</script>

<style>
.text1{
  width:98%;
  min-height:100px;
}
</style>