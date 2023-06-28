<template>
  <canvas id="vue-matrix-raindrop"></canvas>
</template>
<script>
export default {
  name: 'vue-matrix-raindrop',
  props: {
    canvasWidth: {
      type: Number,
      default: 800
    },
    canvasHeight: {
      type: Number,
      default: 600
    },
    fontSize: {
      type: Number,
      default: 20
    },
    fontFamily: {
      type: String,
      default: 'arial'
    },
    textContent: {
      type: String,
      default: '王袁一琦十选登顶年少为'
    },
    textColor: {
      type: String,
      default: '#0F0',
      validator: function (value) {
        // 颜色的正则 以#开头 一个16进制字符 6位或3位
        var colorReg = /^#([0-9a-fA-F]{6})|([0-9a-fA-F]{3})$/g
        return colorReg.test(value)
      }
    },
    backgroundColor: {
      type: String,
      default: 'rgba(0,0,0,0.1)',
      validator: function (value) {
        // RGBA()
        var reg = /^[rR][gG][Bb][Aa][(]((2[0-4][0-9]|25[0-5]|[01]?[0-9][0-9]?),){2}(2[0-4][0-9]|25[0-5]|[01]?[0-9][0-9]?),?(0\.\d{1,2}|1|0)?[)]{1}$/;
        return reg.test(value);
      }
    },
    speed: {
      type: Number,
      default: 10,
      validator: function (value) {
        return value % 1 === 0;
      }
    }
  },
  mounted: function () {
    this.initRAF();
    this.initCanvas();
    this.initRainDrop();
    this.animationUpdate();
  },
  methods: {
    // 初始化 请求动画帧
    initRAF() {
      // 执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行。
      // 它返回一个 long 整数，表示定时器的编号，请求 ID，是回调列表中唯一的标识，这个值可以传递给 cancelAnimationFrame 用于取消这个函数的执行
      // 浏览器兼容性
      window.requestAnimationFrame = (function () {
        return window.requestAnimationFrame ||
          window.webkitRequestAnimationFrame ||
          window.mozRequestAnimationFrame ||
          window.oRequestAnimationFrame ||
          function (callback) {
            window.setTimeout(callback, 1000 / 60);
          };
      })();
      // 取消一个先前通过调用 window.requestAnimationFrame() 方法添加到计划中的动画帧请求。
      window.cancelAnimationFrame = (function () {
        return window.cancelAnimationFrame ||
          window.webkitCancelAnimationFrame ||
          window.mozCancelAnimationFrame ||
          window.oCancelAnimationFrame ||
          function (id) {
            window.clearTimeout(id);
          };
      })();

    },
    // 初始化 画布
    initCanvas() {
      this.canvas = document.getElementById('vue-matrix-raindrop');
      //需要判断获取到的canvas是否是真的canvas
      // Element.tagName 返回当前元素的标签名 字符串值转为小写形式
      if (this.canvas.tagName.toLowerCase() !== 'canvas') {
        console.error("Error! Invalid canvas! Please check the canvas's id!")
      }
      this.canvas.width = this.canvasWidth;
      this.canvas.height = this.canvasHeight;
      this.canvasCtx = this.canvas.getContext('2d');//建立一个 CanvasRenderingContext2D 二维渲染上下文
      console.log(this.canvasCtx,'canvasCtx')
      this.canvasCtx.font = this.fontSize + 'px ' + this.fontFamily;
      this.columns = this.canvas.width / this.fontSize;
    },
    initRainDrop() {
      for (var i = 0; i < this.columns; i++) {
        this.rainDropPositionArray.push(0);
      }
    },
    // 动画更新
    animationUpdate() {
      this.speedCnt++;
      //speed为1最快，越大越慢
      if (this.speedCnt === this.speed) {
        this.speedCnt = 0;
        //绘制背景
        this.canvasCtx.fillStyle = this.backgroundColor;
        this.canvasCtx.fillRect(0, 0, this.canvas.width, this.canvas.height);
        //绘制文字
        this.canvasCtx.fillStyle = this.textColor;
          for (var i = 0, len = this.rainDropPositionArray.length; i < len; i++) {
          this.rainDropPositionArray[i]++;
          var textYPostion = this.rainDropPositionArray[i] * this.fontSize;
          // console.log((textYPostion/this.fontSize)%7)
          var randomText = this.textContent[(textYPostion/this.fontSize)%11];
          this.canvasCtx.fillText(randomText, i * this.fontSize, textYPostion);
          if (textYPostion > this.canvasHeight) {
            if (Math.random() > 0.9) {
              this.rainDropPositionArray[i] = 0;
            }
          }
        }
      }
      window.requestAnimationFrame(this.animationUpdate)
    }
  },
  data() {
    return {
      canvasCtx: null,
      canvas: null,
      columns: 0,
      rainDropPositionArray: [],
      speedCnt: 0
    }
  }
}
</script>

<style scoped></style>