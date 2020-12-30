<template>
  <div id="app">
    <carousel :imageSrc="imageSrc" changeSpeed="20" intervalTime="5000"></carousel>
    <dropDown></dropDown>
    <p>xxxxxxxx</p>
  </div>
</template>

<script>
import carousel from './components/carousel/index'
import dropDown from './components/dropDowns/index'
export default {
  name: 'App',
  data(){
    return {
      imageSrc: ['../static/images/1.jpeg','../static/images/2.jpeg','../static/images/3.jpeg']
    }
  },
  components: {
    carousel,
    dropDown
  }
}
</script>

<style>
body{
  margin: 0;
  padding: 0;
}
</style>
