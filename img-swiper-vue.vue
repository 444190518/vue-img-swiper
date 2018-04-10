/** 
 * @author: zhazhjie 
 * @email: zhazhjie@vip.qq.com
 * @date: 2018-04-10 09:57:09 
 * @version: 1.0 
 */

<template>
  <div id="img-swpier" @mouseenter='showBtn' @mouseleave='hideBtn' :style='{width:width,height:height}'>
    <transition-group :name="toLeft?'list-left':'img-list'" tag="div" style='width:100%;height:100%'>
      <img class="img-list-item" v-for='(item,index) in imgList' :src="item" :key='index' v-show='imgShow==index'>
    </transition-group>
    <ul class="indicator-wrap">
      <li v-for='(item,index) in imgList' @click='selectImg(index)'><span :class='imgShow==index?"select":""'></span></li>
    </ul>
    <div class="left-btn btn" v-show='btnShow' @click='leftImg'>&lt;</div>
    <div class="right-btn btn" v-show='btnShow' @click='rightImg'>&gt;</div>
  </div>
</template>
<script>
export default {
  props:{
  	imgList:{
  		type:Array,  
  	},
    delay:{
      type:Number,
      default:3000
    },
    width:{
      type:String,
      default:'320px'
    },
    height:{
      type:String,
      default:'320px'
    }
  },
  data() {
    return {
      tId: '',
      imgShow: 0,
      btnShow: false,
      toLeft: false
    }
  },
  components: {

  },
  methods: {
    selectImg(i) {
      this.imgShow = i
    },
    imgCarousel() {
      this.rightImg();
      if(this.imgList.length>=2)
        this.tId = setTimeout(this.imgCarousel, this.delay)
    },
    showBtn() {
      clearTimeout(this.tId);
      this.btnShow = true;
    },
    hideBtn() {
      this.toLeft = false;
      this.tId = setTimeout(this.imgCarousel, this.delay);
      this.btnShow = false;
    },
    leftImg() {
      this.toLeft = true;
      if (this.imgShow == 0) {
        this.imgShow = this.imgList.length
      }
      this.imgShow--
    },
    rightImg() {
      this.toLeft = false;
      if (this.imgShow == this.imgList.length-1) {
        this.imgShow = -1
      }
      this.imgShow++
    }
  },
  computed: {

  },
  mounted() {
    this.tId = setTimeout(this.imgCarousel, this.delay);
  }
}

</script>
<style scoped>
#img-swpier {
  position: relative;
  z-index: 2;
  overflow: hidden;
}

#img-swpier img {
  width: 100%;
  height: 100%;
}

.img-list-leave-to {
  transform: translateX(-100%);
}

.img-list-enter {
  transform: translateX(100%);
}

.img-list-enter-active,
.img-list-leave-active {
  position: absolute;
  transition: all 0.8s;
}

.list-left-leave-to {
  transform: translateX(100%);
}

.list-left-enter {
  transform: translateX(-100%);
}

.list-left-enter-active,
.list-left-leave-active {
  position: absolute;
  transition: all 0.8s;
}

.indicator-wrap {
  position: absolute;
  width: 100%;
  bottom: 0;
  left: 0;
  text-align: center;
}

.indicator-wrap li {
  display: inline-block;
  height: 20px;
  line-height: 10px;
  cursor: pointer;
}

.indicator-wrap li span {
  width: 25px;
  height: 3px;
  background: rgba(0, 0, 0, 0.2);
  display: inline-block;
}

.indicator-wrap li:hover span {
  background: rgba(0, 0, 0, 0.4);
}

.indicator-wrap li span.select {
  background: rgba(255, 255, 255, 0.8)
}

.indicator-wrap li:nth-child(n+2) {
  margin-left: 8px;
}

.btn {
  background: rgba(0, 0, 0, 0.2);
  width: 40px;
  height: 40px;
  border-radius: 50%;
  position: absolute;
  top: 50%;
  margin-top: -20px;
  cursor: pointer;
  line-height: 40px;
  text-align: center;
  color: white;
  font-size: 25px;
}

.left-btn {
  left: 5px;
}

.right-btn {
  right: 5px;
}

.btn:hover {
  background: rgba(0, 0, 0, 0.4);
}

</style>
