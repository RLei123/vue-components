<template>
  <div class="process" :class="[status?`is-${status}`:'']">
    <div class="process-bar">
      <div class="process-bar__outer" :style="{height:strokeWidth+'px'}">
        <div class="process-bar__inner" :style="barStyle">
          <div v-if="textInside&&showText" class="process-bar__innerText">{{percentage}}%</div>
        </div>
      </div>
    </div>
    <div v-if="!textInside &&showText" class="process__text" :style="textStyle">
      <template v-if="!status">{{percentage}}%</template>
      <i v-else class="icon" :class="iconClass"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: "",
  props: {
    //宽度
    strokeWidth: {
      type: Number,
      default: 6
    }, //百分比
    percentage: {
      type: Number,
      required: true,
      default: 0,
      validator: value => value >= 0 && value <= 100
    }, //状态 success/exception
    status: {
      type: String
    }, //进度条类型 circle/line
    type: {
      type: String,
      default: "line",
      validator: val => ["circle", "line"].includes(val)
    }, //文字是否内显
    textInside: {
      type: Boolean,
      default: false
    }, //是否显示文字
    showText: {
      type: Boolean,
      default: true
    }, //进度条颜色
    color: {
      type: String
    }
  },
  computed: {
    //计算文字字体大小
    processTextSize() {
      return 12 + this.strokeWidth * 0.4;
    }, //计算进度条颜色
    stroke() {
      if (this.color) return this.color;
      let color;
      switch (this.status) {
        case "success":
          color = "#13ce66";
          break;
        case "exception":
          color = "#ff4949";
          break;
        default:
          color = "#20a0ff";
      }
      return color;
    }, //进度条样式
    barStyle() {
      return {
        width: this.percentage + "%",
        backgroundColor: this.stroke
      };
    }, //文字样式
    textStyle() {
      return {
        fontSize: this.processTextSize + "px",
        marginLeft: "10px",
        display: "inline-block"
      };
    }, //计算文字处图标class名称
    iconClass() {
      if (this.type == "line") {
        return this.status == "success" ? "icon-circle-check" : "icon-circle-close";
      } else
        return this.status == "success"
          ? "icon-check"
          : "icon-close";
    }
  }
};
</script>

<style scoped>
@font-face {
  font-family: "icon"; /* project id 1354837 */
  src: url("./imgs/icons/iconfont.eot");
  src: url("./imgs/icons/iconfont.eot?#iefix") format("embedded-opentype"),
    url("./imgs/icons/iconfont.woff2") format("woff2"),
    url("./imgs/icons/iconfont.woff") format("woff"),
    url("./imgs/icons/iconfont.ttf") format("truetype"),
    url("./imgs/icons/iconfont.svg#iconfont") format("svg");
}

.icon {
  font-family: "icon" !important;
  font-size: 16px;
  font-style: normal;
}

.icon-circle-check::before {
  content: "\e602";
}

.icon-circle-close::before {
  content: "\e630";
}

.icon-check::before {
  content: "\e609";
}

.icon-close::before {
  content: "\e61f";
}

.process.is-success .process__text {
  color: #67c23a;
}

.process.is-exception .process__text {
  color: #f56c6c;
}

.process-bar {
  display: inline-block;
  box-sizing: border-box;
  width: 100%;
  padding-right: 50px;
  margin-right: -50px;
}

.process-bar__outer {
  border-radius: 100px;
  background-color: #ebeef5;
}

.process-bar__inner {
  height: 100%;
  border-radius: 100px;
  transition: width 0.6s ease;
  text-align: right;
  line-height: 1;
}

.process-bar__innerText {
  color: #fff;
  vertical-align: middle;
  display: inline-block;
  font-size: 12px;
  margin: 0 5px;
}
.process-text {
  margin-left: 10px;
  display: inline-block;
  color: #606266;
}
</style>