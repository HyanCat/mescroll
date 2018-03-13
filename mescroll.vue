/*
 * mescroll 滚动
 * @Author: liangzc 
 * @Date: 2018-03-13 09:59:44 
 * @Last Modified by: liangzc
 * @Last Modified time: 2018-03-13 11:37:19
 */
<template>
  <div :id="id"
    class="mescroll"
    ref="mescroll">
    <span>
      <slot/>
    </span>
  </div>
</template>

<script type="text/babel">
export default {
  name: 'me-scroll',
  props: {
    /**
     * 滚动区域Id
     */
    id: {
      type: String,
      default: 'mescroll'
    },
    /**
     * 下拉参数
     */
    optDown: {
      type: Object,
      default: () => {}
    },
    /**
     * 下拉回调
     */
    downCallback: {
      type: Function
    },
    /**
     * 上拉参数
     */
    optUp: {
      type: Object,
      default: () => {}
    },
    /**
     * 上拉回调
     */
    upCallback: {
      type: Function
    },
    /**
     * 统一回调
     */
    callback: {
      type: Function
    }
  },
  data() {
    return {
      instance: null
    };
  },
  mounted() {
    if (!window._Mescroll) {
      require('./mescroll.min.css');
      window._Mescroll = require('./mescroll.min.js');
    }
    if (this.optUp) {
      this.optUp.isBounce =
        typeof this.optUp.isBounce !== 'undefined' ?
          this.optUp.isBounce :
          false;
      this.optUp.callback =
        typeof this.optUp.callback !== 'undefined' ?
          this.optUp.callback :
          this.upCallback || this.callback;
      let empty = this.optUp.empty || {};
      empty.warpId =
        typeof empty.warpId !== 'undefined' ? empty.warpId : this.id;
      this.optUp.empty = empty;
    }

    this.optDown &&
      (this.optDown.callback =
        typeof this.optDown.callback !== 'undefined' ?
          this.optDown.callback :
          this.downCallback || this.callback);

    this.instance = new window._Mescroll(this.id, {
      down: this.optDown,
      up: this.optUp
    });
  },
  methods: {}
};
</script>
<style lang="scss">
.mescroll {
  position: fixed;
}
</style>
