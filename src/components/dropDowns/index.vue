<template>
  <div class="container">
      <div class="choose">
          <div class="text">{{content}}</div>
          <div class="narrow" @click="gradientShow">&gt;</div>
      </div>
      <div class="choice" v-show="show">
          <div class="item" v-for="(item, index) in l" :key="index">{{item}}</div>
      </div>
  </div>
</template>

<script>
export default {
    name: 'dropDown',
    props: {
        content: {
            type: String,
            default: 'dropDown'
        },
        choice: {
            type: Array
        }
    },
    data() {
      return {
          show:true,
          hShow: 0,
          l: []
      }
    },
    methods: {
        immediateShow(){
            this.show = !this.show
        },
        gradientShow(){
            let timer = setInterval(()=>{
                this.hShow+=1
                console.log(this.hShow)
                if(this.hShow===100*this.l.length){
                    clearInterval(timer)
                    return
                }
            },10)
        }
    },
    mounted() {
        this.l = ['choice1','choice2','choice3']
    }
}
</script>

<style scoped>
.container{
    width: 50%;
    position: relative;
    /* border-radius: 2px; */
}
.choose{
    display: flex;
    background-color: grey;
    text-align: center;
    color: white;
}
.choose .text{
    width: 80%;
}
.choose .narrow{
    width: 20%;
}
.choose .narrow:hover{
    cursor: pointer;
}
.choice{
    position: absolute;
    width: 100%;
    background-color: white;
    overflow: hidden;
}
.choice .item{
    border: 1px solid grey;
    text-align: center;
}
</style>
