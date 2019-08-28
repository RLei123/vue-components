<template>
  <div class="puzzle" :style="{width:width+'px',height:height+'px'}">
    <div
      class="puzzle__block"
      v-for="(item,idx)  in blockPoints"
      :key="item.id"
      :style="{
        width:blockWidth+'px',
        height:blockHeight+'px',
        left:item.x+'px',
        top:item.y+'px',
        backgroundImage:`url(${img})`,
        backgroundPosition:`-${correctPoints[idx].x}px -${correctPoints[idx].y}px`,
        opacity:idx===blockPoints.length-1&&0
      }"
      @click="handleClick"
      :ref="idx===blockPoints.length-1?'empty':'block'"
      :data-correctX="correctPoints[idx].x"
      :data-correctY="correctPoints[idx].y"
    ></div>
  </div>
</template>

<script>
export default {
  props: {
    width: {
      type: Number,
      default: 500
    },
    height: {
      type: Number,
      default: 500
    },
    row: {
      type: Number,
      default: 3
    },
    col: {
      type: Number,
      default: 3
    },
    img: {
      type: String,
      required: true
    }
  },
  computed: {
    blockWidth() {
      return this.width / this.col;
    },
    blockHeight() {
      return this.height / this.row;
    },
    correctPoints() {
      const { row, col, blockWidth, blockHeight } = this;
      const arr = [];
      for (let r = 0; r < row; r++) {
        for (let c = 0; c < col; c++) {
          arr.push({
            x: c * blockWidth,
            y: r * blockHeight,
            id: new Date().getTime() + Math.random() * 100
          });
        }
      }
      return arr;
    },
    blockPoints() {
      const points = this.correctPoints;
      const len = points.length;
      const lastEle = points[len - 1];
      const newArr = [...points];
      newArr.length = len - 1;
      newArr.sort(() => Math.random() - 0.5);
      newArr.push(lastEle);

      return newArr;
    }
  },
  methods: {
    //方框交换位置
    handleClick(e) {
      const blockDom = e.target;
      const emptyDom = this.$refs.empty[0];
      if (!this.isAdjacent(blockDom, emptyDom)) return;
      const { left, top } = blockDom.style;
      blockDom.style.left = emptyDom.style.left;
      blockDom.style.top = emptyDom.style.top;
      emptyDom.style.left = left;
      emptyDom.style.top = top;
      //检查是否完成游戏
      const winFlag = this.checkWin();
      if (winFlag) this.winGame(emptyDom);
    }, //判断是否可以交换位置，当位置相邻时可以交换
    isAdjacent(blockDom, emptyDom) {
      const { left: bLeft, top: bTop, width, height } = blockDom.style;
      const { left: eLeft, top: eTop } = emptyDom.style;
      const xDis = Math.floor(Math.abs(parseFloat(bLeft) - parseFloat(eLeft)));
      const yDis = Math.floor(Math.abs(parseFloat(bTop) - parseFloat(eTop)));
      const flag =
        (bLeft == eLeft && yDis == parseInt(height)) || //在同一列，且top相差一个block高度
        (bTop == eTop && xDis == parseInt(width)); //在同一行，且left相差一个block宽度

      return flag;
    }, //是否完成
    checkWin() {
      const bDomArr = this.$refs.block;
      return bDomArr.every(dom => {
        const { left: domLeft, top: domTop } = dom.style;
        const { correctx: correctX, correcty: correctY } = dom.dataset;
        const flag =
          parseInt(domLeft) === parseInt(correctX) &&
          parseInt(domTop) === parseInt(correctY);
        return flag;
      });
    },
    winGame(empty) {
      setTimeout(() => {
        alert("win");
        empty.style.opacity = 1;
        setTimeout(() => {
          this.gotoNext();
        }, 300);
      }, 300);
    },
    gotoNext() {
      const flag = window.confirm("是否继续？");
      if (flag) {
        this.$emit('next');
      }
    }
  }
};
</script>

<style scoped>
.puzzle {
  position: relative;
  border: 2px solid #ccc;
}

.puzzle__block {
  box-sizing: border-box;
  position: absolute;
  border: 1px solid #fff;
  transition: all 0.3s;
}
</style>