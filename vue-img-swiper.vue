/** 
 * @author: zhazhjie 
 * @email: zhazhjie@vip.qq.com
 * @date: 2018-04-10 09:57:09 
 * @version: 1.0 
 */

<template>
  <div id="img-swpier" @mouseenter='showBtn' @mouseleave='hideBtn' :style='{width:width,height:height}'>
    <transition-group :name="cut&&!backward?'move-fast':backward?'move-left':'move-right'" tag="div" style='width:100%;height:100%' @after-leave="afterLeave" @before-enter="beforeEnter">
      <img v-for='(item,index) in imgList' :src="item" :key='index' v-show='imgIndex==index'>
    </transition-group>
    <ul class="indicator-wrap">
      <li v-for='(item,index) in imgList' @click='selectImg(index)'><span :class='imgIndex==index?"select":""'></span></li>
    </ul>
    <transition name='fade-left'>
	    <div class="left-btn btn" v-show='btnFlag' @click='leftMove'></div>
	</transition>
	<transition name='fade-right'>
	    <div class="right-btn btn" v-show='btnFlag' @click='rightMove'></div>
	</transition>
  </div>
</template>
<script>
export default {
  props:{
  	imgList:{
  		type:Array,  
      required:true
  	},
    delay:{
      type:Number,
      default:3000
    },
    width:{
      type:String,
      default:'500px'
    },
    height:{
      type:String,
      default:'300px'
    }
  },
  data() {
    return {
      tId: '',
      imgIndex: 0,
      btnFlag: false,
      backward: false,
      next:true,
      cut:false,
    }
  },
  components: {

  },
  methods: {
  	beforeEnter(){
  		this.next=false;
  	},
  	afterLeave(){
  		this.next=true;
  	},
    selectImg(i) {
      if(!this.next)
        return;
      this.imgIndex = i
    },
    imgCarousel() {
      this.rightMove();
      if(this.imgList.length>=2)
        this.tId = setTimeout(this.imgCarousel, this.delay)
    },
    showBtn() {
      clearTimeout(this.tId);
      this.btnFlag = true;
      this.cut=true;
    },
    hideBtn() {
      this.backward = false;
      this.btnFlag = false;
      this.cut=false;
      this.tId = setTimeout(this.imgCarousel, this.delay);
    },
    leftMove() {
    	if(!this.next)
    		return;
      this.backward = true;
      if (this.imgIndex == 0) {
        this.imgIndex = this.imgList.length
      }
      this.imgIndex--
    },
    rightMove() {
    	if(!this.next)
    		return;
      this.backward = false;
      if (this.imgIndex == this.imgList.length-1) {
        this.imgIndex = -1
      }
      this.imgIndex++
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
  overflow: hidden;
  user-select: none;
}

#img-swpier img {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0
}

.move-right-leave-to,
.move-left-enter,
.move-fast-enter {
  transform: translateX(-100%);
}

.move-right-enter,
.move-left-leave-to,
.move-fast-leave-to {
  transform: translateX(100%);
}

.fade-left-leave-to,
.fade-left-enter {
  transform: translateX(-100%);
  opacity: 0;
}

.fade-right-leave-to,
.fade-right-enter {
  transform: translateX(100%);
  opacity: 0;
}

.move-right-enter-active,
.move-right-leave-active {
  transition: all 1s;
}

.move-fast-enter-active,
.move-fast-leave-active,
.move-left-enter-active,
.move-left-leave-active,
.fade-left-enter-active,
.fade-left-leave-active,
.fade-right-enter-active,
.fade-right-leave-active {
  position: absolute;
  transition: all 0.3s;
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
  transition: all 0.3s;
}

.indicator-wrap li:hover span {
  transition: all 0.3s;
  background: rgba(0, 0, 0, 0.4);
}

.indicator-wrap li span.select {
  background: rgba(255, 255, 255, 0.8)
}

.indicator-wrap li:nth-child(n+2) {
  margin-left: 8px;
}

.btn {
  background-color: rgba(0, 0, 0, 0.2);
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
  transition: all 0.3s;
}

.left-btn {
  left: 10px;
  background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACgAAAAoCAYAAACM/rhtAAABc0lEQVRYR+3YPU4DMRAF4PcOwE0iDgBNIiEFcQtKOkpuQElHB2dJhWjgMAhBoJloJK9kWbvJjj02lsiW+XE+vxlv7CU6v9i5D0dgaYX+d4IiogGckPzITbJaggH3BGABYEnyKwdZE/gM4DqgNgDWJH+tyCpAEXkEcBNhfgCckXz/c+AE7pKkpmi+XBP0xuls3IA1cG5AEXkAcJv0XHZZ4z4oTnAE9w3gKrfn0iYtAk7gLki+mFfDxBeygS1w2T0oIvcA7qJJa1ldkxvGNifYEmdOsDXOBBzBfYb/V7cFMbZOZpe4e6DOrusSD/G3Rs4ucdwfre6BpkWSNnArZFaCUbnTTYL7DbsIGBZOVWQxMCDTLb5bki7APcjibZcbcAKph6WijasrsAbSHRiQ8ZlYX9Ikz0m+WTeytYA6rj5VGA7urwBWJLddAEOKA/I0PPrQ3Y/5qpJgdCPv9+GROSrvQ5MX4NA4VUt86MfnvH8Ezklp32d2Q1KuKYQ6wEgAAAAASUVORK5CYII=');
}
.right-btn {
  right: 10px;
  background-image: url('data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACgAAAAoCAYAAACM/rhtAAABRklEQVRYR+3YQUoDMRQG4P8/Qd3ormAvVbrRjXbTa7R3kLoQN+rGg9iepj1A+SUQShk6A8l7T4LMrDPMx/8yyUuIxh827sMItFbofycoaULyaE1p6P3qBCVNAewArEm+RCGrgJJuAewB3GfYKgpZC1wA+OqkFoKsAiaYpCWA1w7yieSbZ7mrgT1IAXj2RJqAF8gtcF70XZFmYEY+AniPQLoAB5APJD8sc9INGIV0BUYg3YHeyBDgAHJO8rtkToYBe5AnAIsSZCjQA/kXwLRvf16skUUphgIlmXCpAmFASdd2lzZ+kh5c1a7inqAnzr3E3jhXYATODdiDc2lczXMwt/5tNqxXziWu3bSpxE0fmvIO0e6xU9IdgB8AsyYP7rlLSVcfCbmJulUwzcGMvCF5KGlAS8eal5nSD5aOH4GliXXHjwlaE/wFy/moKRlZXVUAAAAASUVORK5CYII=');
}

.btn:hover {
	transition: all 0.3s;
  background-color: rgba(0, 0, 0, 0.4);
}
</style>
