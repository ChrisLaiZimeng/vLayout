<template>
    <div class="container" :style="{'width': width,'border-radius': borderRadius}">
        <div class="imgContainer" 
        v-for="(item, index) in imageList" :key="index"
        :style="{'right': pos, 'border-radius': borderRadius}">
            <img :src="item" :style="{'border-radius': borderRadius}">
        </div>
        <div class="keys" v-if="btn">
            <div class="box">
                <div id="prev">&lt;</div>
                <div id="next" @click="next">&gt;</div>
            </div>
        </div>
        <div class="points" v-if="dot">
            <div class="box">
                <div class="point" :class="{'pointNow': index===nowSwiper}" v-for="(item, index) in imageSrc" :key="index"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'carousel',
    props: {
        imageSrc: {
            type: Array,
            default: []
        },
        width: {
            type: String,
            default: '60%'
        },
        borderRadius: {
            type: String,
            default: '8px'
        },
        changeSpeed: {
            type: String,
            default: '20'
        },
        intervalTime: {
            type: String,
            default: '5000'
        },
        leftPos: {
            type: String,
            default: '50%'
        },
        dot: {
            type: Boolean,
            default: true
        },
        btn: {
            type: Boolean,
            default: true
        }
    },
    data() {
      return {
          imageList: [],
          len: 0,
          pos:'100%',
          nowSwiper: 0,
          timer: '',
          bigTimer: ''
      }
    },
    methods: {
        _initSrc(){
            this.imageList.push(this.imageSrc[this.imageSrc.length-1])
            this.imageList.push(...this.imageSrc)
            this.len = this.imageList.length
            // console.log(this.imageList)
        },
        changeBanner(){
            // let timer 
            this.timer = setInterval(()=>{
                let num = parseInt(this.pos.substring(0,this.pos.length-1))
                if(num%100 === 0){
                    if(num===(this.len-1)*100){
                        this.pos = '0%'
                    }
                    if(this.nowSwiper===this.len-2){
                        this.nowSwiper = 0
                    } else {
                        this.nowSwiper++
                    }
                    clearInterval(this.timer)
                    return
                }
                let now = num+5
                this.pos = now+'%'
            },this.changeSpeed)
        },
        next(){
            clearInterval(this.bigTimer)
            clearInterval(this.timer)
            let nowLeft = parseInt(this.pos.substring(0,this.pos.length-1))
            let nextLeft = (1+Math.floor(nowLeft/100))*100
            let once = setInterval(()=>{
                let num = parseInt(this.pos.substring(0,this.pos.length-1))
                if(num === nextLeft){
                    if(num===(this.len-1)*100){
                        this.pos = '0%'
                    }
                    if(this.nowSwiper===this.len-2){
                        this.nowSwiper = 0
                    } else {
                        this.nowSwiper++
                    }
                    clearInterval(once)
                    return
                }
                let now = num+5
                this.pos = now+'%'
            },this.changeSpeed)
            this.resetInterval()
        },
        resetInterval(){
            this.bigTimer = setInterval(()=>{
                let num = parseFloat(this.pos.substring(0,this.pos.length-1))
                this.pos = (num+5)+'%'
                this.changeBanner()
            },this.intervalTime)
        }
    },
    created() {
        this._initSrc()
    },
    mounted() {
        let container = document.getElementsByClassName('container')[0]
        let left = parseInt(this.leftPos.substring(0,this.leftPos.length-1))
        let points = document.getElementsByClassName('points')[0]
        container.style.width = this.width
        container.style.left = this.leftPos
        container.style.transform = "translateX(-" + left + '%)'
        points.style.width = (this.len-1)*5 + '%'

        this.resetInterval()
    }
}
</script>

<style scoped>
/* according to width */
.container{
    position: relative;
    display: flex;
    overflow: hidden;
}
.imgContainer{
    width: 100%;
    flex-shrink: 0;
    position: relative;
    right: 100%;
}
img{
    max-width: 100%;
    display: block;
}
.keys{
    position: absolute;
    height: 5%;
    width: 94%;
    left: 0;
    right: 0;
    bottom: 0;
    top: 0;
    padding: 0 3%;
    margin: auto;
}
#prev, #next{
    color: white;
}
#prev:hover, #next:hover{
    cursor: pointer;
}

.points{
    position: absolute;
    height: 5%;
    /* width: 10%; */
    left: 0;
    right: 0;
    bottom: 5%;
    margin: 0 auto;
}
.box{
    width: 100%;
    display: flex;
    justify-content: space-between;
}
.point{
    width: .5em;
    height: .5em;
    background-color: gainsboro;
    border-radius: 50%;
    opacity: 0.6;
}
.pointNow{
    background-color: gray;
}
</style>
