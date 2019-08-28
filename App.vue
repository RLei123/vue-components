<template>
  <div>
    <Puzzle v-bind="puzzleConfig[level]" @next="handlerNext"></Puzzle>

    <!-- <h3>进度条 -- 状态显示</h3>
        <d-process :percentage="30" status="exception"></d-process><br>
        <d-process :percentage="50" status="success"></d-process><br>
        <h3>进度条 -- 文字外显</h3>
        <d-process :percentage="0"></d-process><br>
        <d-process :percentage="30"></d-process><br>
        <d-process :percentage="50"></d-process><br>
        <d-process :percentage="70"></d-process><br>
        <h3>进度条 -- 文字内显</h3>

        <d-process :strokeWidth="18" :showText="false" :percentage="26" textInside="true"></d-process>
        <d-process :strokeWidth="18" color="orange" :percentage="36" textInside="true"></d-process>
        <d-process :strokeWidth="18" :percentage="46" textInside="true"></d-process>
        <d-process :strokeWidth="18" :percentage="56" textInside="true"></d-process>
    <d-process :strokeWidth="18" :percentage="66" textInside="true"></d-process>-->
  </div>
</template>

<script>
import DProcess from "./Process";
import Puzzle from "./Puzzle";
import DForm from "./DForm";

export default {
  components: {
    DProcess,
    Puzzle,
    DForm
  },
  data() {
    return {
      puzzleConfig: [
        { row: 3, col: 3, img: "./puzzleImg/1.jpg" },
        { row: 4, col: 4, img: "./puzzleImg/2.jpg" },
        { row: 5, col: 5, img: "./puzzleImg/3.jpg" },
        { row: 6, col: 6, img: "./puzzleImg/4.jpg" },
        { row: 7, col: 7, img: "./puzzleImg/5.jpg" }
      ],
      level: 0
    };
  },
  methods: {
    handlerNext() {
      if (++this.level == this.puzzleConfig.length) {
        const flag = window.confirm("已经是最后一关了，重新开始？");
        if (flag) this.level = 0;
      }
    }
  }
};
</script>

<style scoped>
</style>