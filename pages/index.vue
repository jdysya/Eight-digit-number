<template>
  <div>
    <el-button @click="stateChange()">Click Me</el-button>

    <div style="display: flex; margin-top: 20px; height: 100px;">
      <transition name="el-zoom-in-center">
        <div @click="swapSpace(0)"
          :class="{ 'transition-box': true, 'roll-in-left': showArray[0][0], 'roll-in-right': showArray[0][1], 'roll-in-top': showArray[0][2], 'roll-in-bottom': showArray[0][3] }">
          {{ numShow(0) }}</div>
      </transition>

      <transition name="el-zoom-in-top">
        <div @click="swapSpace(1)"
          :class="{ 'transition-box': true, 'roll-in-left': showArray[1][0], 'roll-in-right': showArray[1][1], 'roll-in-top': showArray[1][2], 'roll-in-bottom': showArray[1][3] }">
          {{ numShow(1) }}</div>
      </transition>

      <transition name="el-zoom-in-bottom">
        <div @click="swapSpace(2)"
          :class="{ 'transition-box': true, 'roll-in-left': showArray[2][0], 'roll-in-right': showArray[2][1], 'roll-in-top': showArray[2][2], 'roll-in-bottom': showArray[2][3] }">
          {{ numShow(2) }}</div>
      </transition>
    </div>

    <div style="display: flex; margin-top: 20px; height: 100px;">
      <transition name="el-zoom-in-center">
        <div @click="swapSpace(3)"
          :class="{ 'transition-box': true, 'roll-in-left': showArray[3][0], 'roll-in-right': showArray[3][1], 'roll-in-top': showArray[3][2], 'roll-in-bottom': showArray[3][3] }">
          {{ numShow(3) }}</div>
      </transition>

      <transition name="el-zoom-in-top">
        <div @click="swapSpace(4)"
          :class="{ 'transition-box': true, 'roll-in-left': showArray[4][0], 'roll-in-right': showArray[4][1], 'roll-in-top': showArray[4][2], 'roll-in-bottom': showArray[4][3] }">
          {{ numShow(4) }}</div>
      </transition>

      <transition name="el-zoom-in-bottom">
        <div @click="swapSpace(5)"
          :class="{ 'transition-box': true, 'roll-in-left': showArray[5][0], 'roll-in-right': showArray[5][1], 'roll-in-top': showArray[5][2], 'roll-in-bottom': showArray[5][3] }">
          {{ numShow(5) }}</div>
      </transition>
    </div>

    <div style="display: flex; margin-top: 20px; height: 100px;">
      <transition name="el-zoom-in-center">
        <div @click="swapSpace(6)"
          :class="{ 'transition-box': true, 'roll-in-left': showArray[6][0], 'roll-in-right': showArray[6][1], 'roll-in-top': showArray[6][2], 'roll-in-bottom': showArray[6][3] }">
          {{
              numShow(6)
          }}</div>
      </transition>

      <transition name="el-zoom-in-top">
        <div @click="swapSpace(7)"
          :class="{ 'transition-box': true, 'roll-in-left': showArray[7][0], 'roll-in-right': showArray[7][1], 'roll-in-top': showArray[7][2], 'roll-in-bottom': showArray[7][3] }">
          {{
              numShow(7)
          }}</div>
      </transition>

      <transition name="el-zoom-in-bottom">
        <div @click="swapSpace(8)"
          :class="{ 'transition-box': true, 'roll-in-left': showArray[8][0], 'roll-in-right': showArray[8][1], 'roll-in-top': showArray[8][2], 'roll-in-bottom': showArray[8][3] }">
          {{ numShow(8) }}</div>
      </transition>
    </div>
    <div>当前状态:{{ isSuccess }}</div>
  </div>
</template>

<script>
export default {
  data: () => ({
    // showArray[0]: true,
    initState: [2, 8, 3, 1, 6, 4, 7, 0, 5],
    moveState: [],
    endState: [1, 2, 3, 8, 0, 4, 7, 6, 5],
    showArray: [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]],
    directions: [-1, -3, 1, 3], //空格移动的方向
    aStar: {
      level: 1, //A*算法中,记录当前的层数,即g(x)
      visit: new Set(), //保存已生成过的状态
      currentList: [], // 当前层的状态集合,后续需要选择估价函数最小的进行扩展,若最小值有多个则都需要向下扩展
      currentMin: [] //当前层的估值函数的最小值
    },
    isSuccess: false
  }),
  computed: {

  },
  methods: {
    stateChange() {
      this.getSolution(this.moveState, 1)
      console.log(this.aStar);
    },
    /**
     *  启发式搜索
     * g(x):从初始状态到x需要进行的移动操作的次数
     * h(x):所有棋子与目标位置的曼哈顿距离之和
     * @param {*} toDoState 移动的状态数组
     * @param {*} level 层数
     */
    getSolution(toDoState, level) {

      let pos = this.getPrecentPos(toDoState) //获取空格位置
      //初始状态,将空格按四个方向移动,然后取估价函数最小的继续
      this.directions.forEach(item => {
        let posMoving = pos + item
        if (!this.isLegal(pos, posMoving, level)) return //不合法则跳出本次循环,进入下一轮
      })
      //该层扩展完毕,需要找到较小的估值函数
      console.log(`第${level}扩展完毕,最小值为${this.aStar.currentMin}`);
      level++
      // this.aStar.currentList[level-1].forEach(item => {
      //   if (item.state.valuation == this.aStar.currentMin) {
      //     // 当前结点估价函数最小,进行扩展
      //     this.getSolution(item.state)
      //   }
      // })


    },

    /**
     * 点击空格周围的元素后实现空格移动的效果
     * 若无法移动弹出对应的提示
     * @param {*} index
     */
    swapSpace(index) {
      console.log("现在点击的是第" + index + "个卡片");
      // 先获取当前空格所在的位置
      let space_pos = this.getPrecentPos(this.initState)
      if (Math.abs(index - space_pos) == 1 || Math.abs(index - space_pos) == 3) {
        console.log("可以移动");
        this.swapELe(space_pos, index)
      }
      else {
        this.$alert("当前操作非法!")
      }
    },
    /**
     * 交换两张卡片的位置
     * @param {*} space 空格位置
     * @param {*} nonSpace 非空格位置
     */
    swapELe(space, nonSpace) {
      // 需要判断两个卡片的相对位置,共有四种情况
      if (space - nonSpace == 1) {
        // 空格左边
        this.showArray[nonSpace].splice(1, 1, true)
        this.showArray[space].splice(0, 1, true)
        setTimeout(() => {
          this.showArray[nonSpace].splice(1, 1, false)
          this.showArray[space].splice(0, 1, false)
        }, 600);
      }
      else if (nonSpace - space == 1) {
        // 空格右边
        this.showArray[nonSpace].splice(0, 1, true)
        this.showArray[space].splice(1, 1, true)
        setTimeout(() => {
          this.showArray[nonSpace].splice(0, 1, false)
          this.showArray[space].splice(1, 1, false)
        }, 600);
      }
      else if (space - nonSpace == 3) {
        // 空格上边
        this.showArray[nonSpace].splice(2, 1, true)
        this.showArray[space].splice(3, 1, true)
        setTimeout(() => {
          this.showArray[nonSpace].splice(2, 1, false)
          this.showArray[space].splice(3, 1, false)
        }, 600);
      }
      else if (nonSpace - space == 3) {
        // 空格下边
        this.showArray[nonSpace].splice(3, 1, true)
        this.showArray[space].splice(2, 1, true)
        setTimeout(() => {
          this.showArray[nonSpace].splice(3, 1, false)
          this.showArray[space].splice(2, 1, false)
        }, 600);
      } else {
        this.$alert("出错了!")
      }
      let temp1 = this.initState[space]
      let temp2 = this.initState[nonSpace]
      this.initState.splice(space, 1, temp2)
      this.initState.splice(nonSpace, 1, temp1)

    },
    /**
     * 获取传入状态空格的当前位置
     */
    getPrecentPos(ls) {
      let res = -1
      ls.forEach((item, index) => {
        if (item == 0) res = index
      })
      return res
    },
    /**
     * 判断移动后的空格位置是否合法
     * 若合法,便直接进行移动
     * @param {*} before  移动前的位置
     * @param {*} after 移动后的位置
     */
    isLegal(before, after, level) {
      let res = true
      if (after < 0 || after > 8) return false
      let [...tmpArray] = this.initState //ES6语法,深拷贝
      console.log(tmpArray);

      // 直接将对应元素互换位置即可
      let temp = tmpArray[before]
      tmpArray[before] = tmpArray[after]
      tmpArray[after] = temp

      if (this.aStar.visit.has(tmpArray.join())) {
        // 当前移动的状态已经存在
        res = false
      }
      else {
        // 当前状态合法,将其添加进数组中进行保存
        console.log(`before=${before},after=${after}`);
        let val = level + this.getDistance(tmpArray) //估价函数,当前层数+与目标的曼哈顿距离
        if (!this.aStar.currentList[level - 1]) this.aStar.currentList[level - 1] = []
        this.aStar.currentList[level - 1].push({
          state: tmpArray, //当前状态,数组类型
          valuation: val
        })
        if (!this.aStar.currentMin[level-1] || val < this.aStar.currentMin[level - 1]) this.aStar.currentMin[level-1] = val
        this.aStar.visit.add(tmpArray.join()) // 将当前的数组加入状态集合
      }

      return res

    },

    /**
     * 获取与目标状态的曼哈顿距离
     * @param {*} ls 当前状态数组
     */
    getDistance(ls) {
      let val = 0
      ls.forEach((cur, ind1) => {
        this.endState.forEach((tar, ind2) => {
          if (cur == tar && cur != 0) {
            let tmp = Math.abs(this.getRowCol(ind1).row - this.getRowCol(ind2).row)
              + Math.abs(this.getRowCol(ind1).col - this.getRowCol(ind2).col)
            val += tmp
            console.log(tmp);
            return
          }
        })
      })
      return val
    },


    /**
     * 根据下标获取对应的行和列
     * @param {*} pos
     */
    getRowCol(pos) {
      return {
        row: parseInt(pos / 3),
        col: pos % 3
      }
    },



    /**
     * 0不显示,显示为空
     * @param {*} index
     */
    numShow(index) {
      if (this.initState[index] != 0) {
        return this.initState[index]
      }
      else {
        return ' '
      }
    }
  },
  mounted() {
    [...this.moveState] = this.initState // 深拷贝
    // for (let i = 0; i < 8; i++) this.showArray.push([0, 0, 0, 0])
    // console.log(this.showArray);
    // let tmp_res = this.getDistance([2, 8, 3, 1, 6, 4, 0, 7, 5])
    // console.log(tmp_res);
    // for(let i =0; i < 9; i++){
    //   console.log(this.getRowCol(i));
    // }
  }
}
</script>

<style>
.transition-box {
  margin-bottom: 10px;
  width: 200px;
  height: 100px;
  border-radius: 4px;
  background-color: #409EFF;
  text-align: center;
  color: #fff;
  padding: 40px 20px;
  box-sizing: border-box;
  margin-right: 20px;
}



.jello-horizontal {
  -webkit-animation: jello-horizontal 0.9s both;
  animation: jello-horizontal 0.9s both;
}


/* ----------------------------------------------
 * Generated by Animista on 2022-11-28 13:14:45
 * Licensed under FreeBSD License.
 * See http://animista.net/license for more info.
 * w: http://animista.net, t: @cssanimista
 * ---------------------------------------------- */

/**
 * ----------------------------------------
 * animation jello-horizontal
 * ----------------------------------------
 */
@-webkit-keyframes jello-horizontal {
  0% {
    -webkit-transform: scale3d(1, 1, 1);
    transform: scale3d(1, 1, 1);
  }

  30% {
    -webkit-transform: scale3d(1.25, 0.75, 1);
    transform: scale3d(1.25, 0.75, 1);
  }

  40% {
    -webkit-transform: scale3d(0.75, 1.25, 1);
    transform: scale3d(0.75, 1.25, 1);
  }

  50% {
    -webkit-transform: scale3d(1.15, 0.85, 1);
    transform: scale3d(1.15, 0.85, 1);
  }

  65% {
    -webkit-transform: scale3d(0.95, 1.05, 1);
    transform: scale3d(0.95, 1.05, 1);
  }

  75% {
    -webkit-transform: scale3d(1.05, 0.95, 1);
    transform: scale3d(1.05, 0.95, 1);
  }

  100% {
    -webkit-transform: scale3d(1, 1, 1);
    transform: scale3d(1, 1, 1);
  }
}

@keyframes jello-horizontal {
  0% {
    -webkit-transform: scale3d(1, 1, 1);
    transform: scale3d(1, 1, 1);
  }

  30% {
    -webkit-transform: scale3d(1.25, 0.75, 1);
    transform: scale3d(1.25, 0.75, 1);
  }

  40% {
    -webkit-transform: scale3d(0.75, 1.25, 1);
    transform: scale3d(0.75, 1.25, 1);
  }

  50% {
    -webkit-transform: scale3d(1.15, 0.85, 1);
    transform: scale3d(1.15, 0.85, 1);
  }

  65% {
    -webkit-transform: scale3d(0.95, 1.05, 1);
    transform: scale3d(0.95, 1.05, 1);
  }

  75% {
    -webkit-transform: scale3d(1.05, 0.95, 1);
    transform: scale3d(1.05, 0.95, 1);
  }

  100% {
    -webkit-transform: scale3d(1, 1, 1);
    transform: scale3d(1, 1, 1);
  }
}

.roll-in-right {
  -webkit-animation: roll-in-right 0.6s ease-out both;
  animation: roll-in-right 0.6s ease-out both;
}

/* ----------------------------------------------
 * Generated by Animista on 2022-11-28 13:23:24
 * Licensed under FreeBSD License.
 * See http://animista.net/license for more info.
 * w: http://animista.net, t: @cssanimista
 * ---------------------------------------------- */

/**
 * ----------------------------------------
 * animation roll-in-right
 * ----------------------------------------
 */
@-webkit-keyframes roll-in-right {
  0% {
    -webkit-transform: translateX(300px) rotate(540deg);
    transform: translateX(300px) rotate(540deg);
    opacity: 0;
  }

  100% {
    -webkit-transform: translateX(0) rotate(0deg);
    transform: translateX(0) rotate(0deg);
    opacity: 1;
  }
}

@keyframes roll-in-right {
  0% {
    -webkit-transform: translateX(300px) rotate(540deg);
    transform: translateX(300px) rotate(540deg);
    opacity: 0;
  }

  100% {
    -webkit-transform: translateX(0) rotate(0deg);
    transform: translateX(0) rotate(0deg);
    opacity: 1;
  }
}

.roll-in-left {
  -webkit-animation: roll-in-left 0.6s ease-out both;
  animation: roll-in-left 0.6s ease-out both;
}

/* ----------------------------------------------
 * Generated by Animista on 2022-11-28 13:26:26
 * Licensed under FreeBSD License.
 * See http://animista.net/license for more info.
 * w: http://animista.net, t: @cssanimista
 * ---------------------------------------------- */

/**
 * ----------------------------------------
 * animation roll-in-left
 * ----------------------------------------
 */
@-webkit-keyframes roll-in-left {
  0% {
    -webkit-transform: translateX(-800px) rotate(-540deg);
    transform: translateX(-800px) rotate(-540deg);
    opacity: 0;
  }

  100% {
    -webkit-transform: translateX(0) rotate(0deg);
    transform: translateX(0) rotate(0deg);
    opacity: 1;
  }
}

@keyframes roll-in-left {
  0% {
    -webkit-transform: translateX(-800px) rotate(-540deg);
    transform: translateX(-800px) rotate(-540deg);
    opacity: 0;
  }

  100% {
    -webkit-transform: translateX(0) rotate(0deg);
    transform: translateX(0) rotate(0deg);
    opacity: 1;
  }
}


.roll-in-top {
  -webkit-animation: roll-in-top 0.6s ease-out both;
  animation: roll-in-top 0.6s ease-out both;
}

/* ----------------------------------------------
 * Generated by Animista on 2022-12-5 11:49:33
 * Licensed under FreeBSD License.
 * See http://animista.net/license for more info.
 * w: http://animista.net, t: @cssanimista
 * ---------------------------------------------- */

/**
 * ----------------------------------------
 * animation roll-in-top
 * ----------------------------------------
 */
@-webkit-keyframes roll-in-top {
  0% {
    -webkit-transform: translateY(-800px) rotate(-540deg);
    transform: translateY(-800px) rotate(-540deg);
    opacity: 0;
  }

  100% {
    -webkit-transform: translateY(0) rotate(0deg);
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
}

@keyframes roll-in-top {
  0% {
    -webkit-transform: translateY(-800px) rotate(-540deg);
    transform: translateY(-800px) rotate(-540deg);
    opacity: 0;
  }

  100% {
    -webkit-transform: translateY(0) rotate(0deg);
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
}

.roll-in-bottom {
  -webkit-animation: roll-in-bottom 0.6s ease-out both;
  animation: roll-in-bottom 0.6s ease-out both;
}

/* ----------------------------------------------
 * Generated by Animista on 2022-12-5 11:49:46
 * Licensed under FreeBSD License.
 * See http://animista.net/license for more info.
 * w: http://animista.net, t: @cssanimista
 * ---------------------------------------------- */

/**
 * ----------------------------------------
 * animation roll-in-bottom
 * ----------------------------------------
 */
@-webkit-keyframes roll-in-bottom {
  0% {
    -webkit-transform: translateY(800px) rotate(540deg);
    transform: translateY(800px) rotate(540deg);
    opacity: 0;
  }

  100% {
    -webkit-transform: translateY(0) rotate(0deg);
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
}

@keyframes roll-in-bottom {
  0% {
    -webkit-transform: translateY(800px) rotate(540deg);
    transform: translateY(800px) rotate(540deg);
    opacity: 0;
  }

  100% {
    -webkit-transform: translateY(0) rotate(0deg);
    transform: translateY(0) rotate(0deg);
    opacity: 1;
  }
}
</style>
