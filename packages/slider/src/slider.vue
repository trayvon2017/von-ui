<!--
 * @Description: 
 * @Date: 2023-01-06 17:04:16
 * @LastEditors: cfb
 * @LastEditTime: 2023-02-09 19:27:39
 * @FilePath: /von-ui/packages/slider/src/slider.vue
-->
<template>
  <div
    class="von-slider"
    ref="sliderView"
    @touchstart="handleTouchstart"
    @touchmove="handleTouchmove"
    @touchend="hanldeTouchend"
  >
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "von-slider",
  props: {},
  data() {
    return {
      preX: 0,
      preY: 0,
      direction: 0, // 0代表垂直方向
    };
  },
  mounted() {
    this.startY = 0;
    this.currentHeight = 100;
    this.maxHeight = 200;
    this.minHeight = 100;
    this.initChildEle();
  },
  methods: {
    initChildEle() {
      this.childEle = this.$refs.sliderView.firstElementChild;
      if (this.childEle) {
        this.childEle.style.overflow = "hidden";
        this.childEle.addEventListener("touchstart", this.childTouchHandler);
        this.childEle.addEventListener("touchmove", this.childTouchHandler);
        this.childEle.addEventListener("touchend", this.childTouchHandler);
      }
    },
    childTouchHandler(event) {
      console.log(event);
      if (this.childEle.scrollTop > 0) {
        event.stopPropagation();
      }
    },
    handleTouchstart(e) {
      console.log(e.target);
      this.startY = e.touches[0].clientY;
      this.originHeight = this.currentHeight;
    },
    handleTouchmove(e) {
      if (this.childEle.scrollTop > 0) {
        return;
      }
      // console.log(e);
      // const currentX = e.touches[0].clientX;
      const currentY = e.touches[0].clientY;
      const del = currentY - this.startY;
      const tempHeight = this.originHeight - del;
      if (tempHeight >= this.maxHeight) {
        this.currentHeight = this.maxHeight;
      } else if (tempHeight <= this.minHeight) {
        this.currentHeight = this.minHeight;
      } else {
        this.currentHeight = tempHeight;
      }
      // this.currentHeight ? tempHeight >= this.maxHeight ? tempHeight :
      this.$refs.sliderView.style.height = this.currentHeight + "px";
      console.log(this.currentHeight);
      if (this.currentHeight !== 200) {
        this.childEle.style.overflow = "hidden";
      }
    },
    hanldeTouchend() {
      this.startY = 0;
      if (this.currentHeight === 200) {
        this.childEle.style.overflow = "auto";
      } else if (this.currentHeight < 150) {
        this.$refs.sliderView.style.height = this.minHeight + "px";
      } else if (this.currentHeight >= 150) {
        this.$refs.sliderView.style.height = this.maxHeight + "px";
      }
    },
  },
};
</script>
