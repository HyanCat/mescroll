/*
 * mescroll 滚动
 * @Author: liangzc 
 * @Date: 2018-03-13 09:59:44 
 * @Last Modified by: liangzc
 * @Last Modified time: 2018-03-13 14:07:58
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

    if (!this.downCallback && !this.upCallback && this.callback) {
      if (!this.isEmptyObject(this.upOption)) {
        this.upOption.callback = this.callback;
        this.downOption.use = false;
      } else if (!this.isEmptyObject(this.downOption)) {
        this.downOption.callback = this.callback;
        this.upOption.use = false;
      }
    }
    this.upOption.auto = !(this.downOption.callback && this.upOption.callback);
    this.instance = new window._Mescroll(this.id, {
      down: this.downOption,
      up: this.upOption
    });
  },
  computed: {
    upOption() {
      let optUp = this.optUp || {};
      optUp.isBounce =
        typeof optUp.isBounce !== 'undefined' ? optUp.isBounce : false;
      optUp.callback =
        typeof optUp.callback !== 'undefined' ?
          optUp.callback :
          this.upCallback;
      let empty = optUp.empty || {};
      empty.warpId =
        typeof empty.warpId !== 'undefined' ? empty.warpId : this.id;
      optUp.empty = empty;
      return optUp;
    },
    downOption() {
      let optDown = this.optDown || {};
      optDown.callback =
        typeof optDown.callback !== 'undefined' ?
          optDown.callback :
          this.downCallback;
      return optDown;
    }
  },
  methods: {
    isEmptyObject(obj) {
      let name;
      for (name in obj) {
        return false;
      }
      return true;
    }
  }
};
</script>
<style lang="scss">
.mescroll {
  position: fixed;
}
</style>
