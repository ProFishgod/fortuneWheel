<template>
  <el-scrollbar height="600px">
    <div class="bg-img">
      <img class="bg1" src="./pull/bg1.png" />
      <img class="yp" src="./pull/yp.png" />
      <img class="a" src="./pull/a.png" :style="{ display: act1 }" />
      <img class="b" src="./pull/b.png" :style="{ display: act2 }" />
      <img class="c" src="./pull/c.png" :style="{ display: act3 }" />
      <img class="d" src="./pull/d.png" :style="{ display: act4 }" />
      <img class="e" src="./pull/e.png" :style="{ display: act5 }" />
      <img class="f" src="./pull/f.png" :style="{ display: act6 }" />
      <img class="ypw" src="./pull/ypw.png" />
      <img class="ypz" src="./pull/ypz.png" />
      <img class="ypn" src="./pull/ypn.png" />
      <img class="zx" src="./pull/zx.png" />
      <img class="zz" src="./pull/zz.png" :style="rotateStyle" />
      <img
        class="chou"
        src="./pull/chou.png"
        :disabled="false"
        @click="getRandom(), activate()"
      />
      <img class="jfcj" src="./pull/jfcj.png" />
      <img class="zs" src="./pull/zs.png" />
      <img
        class="lsjl"
        src="./pull/lsjl.png"
        @click=";(dialogTableVisible = true), showResult()"
      />
      <img class="gz" src="./pull/gz.png" />
      <img class="qiua1" src="./pull/qiua.png" />
      <img class="qiua2" src="./pull/qiua.png" />
      <img class="qiub" src="./pull/qiub.png" />
      <img class="qiuc" src="./pull/qiuc.png" />
      <img class="qiud" src="./pull/qiud.png" />
      <img class="qiue" src="./pull/qiue.png" />
      <img class="xxcy1" src="./pull/xxcy.png" />
      <img class="xxcy2" src="./pull/xxcy.png" />
      <img class="icona" src="./pull/icona.png" />
      <img class="iconb" src="./pull/iconb.png" />
      <img class="iconc" src="./pull/iconc.png" />
      <img class="icond" src="./pull/icond.png" />
      <img class="jpa" src="./pull/jpa.png" />
      <img class="jpb" src="./pull/jpb.png" />
      <img class="jpc" src="./pull/jpc.png" />
      <img class="jpd" src="./pull/jpd.png" />
      <img class="ys" src="./pull/ys.png" />
      <p class="yss">{{ ys }}</p>
      <img class="jb" src="./pull/jb.png" />
      <p class="jbs">{{ jb }}</p>
      <div class="animate__fadeInDown" :style="{ display: showResultBoard }">
        <img class="zjts" src="./pull/zjts.png" />
        <p class="zjtsbt">{{ resultTitle }}</p>
        <p class="zjtswz">{{ resultMessage }}</p>
        <img
          class="zjtstp"
          src="./pull/jpa.png"
          :style="{ display: showAwardPic1 }"
        />
        <img
          class="zjtstp"
          src="./pull/jpb.png"
          :style="{ display: showAwardPic2 }"
        />
        <img
          class="zjtstp"
          src="./pull/jpc.png"
          :style="{ display: showAwardPic3 }"
        />
        <img
          class="zjtstp"
          src="./pull/jpd.png"
          :style="{ display: showAwardPic4 }"
        />
        <img
          class="zjtstp"
          src="./pull/jb.png"
          :style="{ display: showAwardPic5 }"
        />
        <img
          class="qra"
          src="./pull/qra.png"
          @mouseover="changeColor = 'block'"
          @mouseleave="changeColor = 'none'"
        />
        <img
          class="qrb"
          src="./pull/qrb.png"
          :style="{ display: changeColor }"
          @click="deactivate()"
        />
        <img
          class="qrc"
          src="./pull/qrc.png"
          @mouseover="changeColor = 'block'"
          @mouseleave="changeColor = 'none'"
          @click="deactivate()"
        />
      </div>
      <!-- <h1 class="score">积分：{{ point.value }}</h1> -->
    </div>
    <!-- <div class="settingBlock">
      <el-affix :offset="120">
        <el-button type="primary">距离顶部 120px</el-button>
      </el-affix>
    </div> -->
    <el-dialog v-model="dialogTableVisible" title="中奖记录">
      <el-table :data="gridData">
        <el-table-column
          property="date"
          label="日期"
          width="550"
        ></el-table-column>
        <el-table-column property="award" label="获得奖项"></el-table-column>
      </el-table>
    </el-dialog>
    <el-dialog v-model="confirmVisible">
      <el-button type="primary">距离顶部 120px</el-button>
    </el-dialog>
  </el-scrollbar>
  <!-- <el-button type="primary" >随机</el-button>
  <el-button type="primary" @click="showResult()">获奖历史</el-button> -->
</template>

<script>
//import { indexOf } from 'lodash'
import { ref } from 'vue'
import './pull/styles.css'
import 'animate.css'

const config = {
  // 总旋转时间
  duration: 4000,
  // 旋转圈数
  circle: 4,
  mode: 'ease-in-out'
}
let choice = 0
let award = [],
  awardTime = []
export default ({
  name:"App",
  setup() {
    var gridData = ref([{ date: '', award: '' }])
    const showResult = () => {
      //console.log('1' + gridData.value.length)
      //console.log('point' + typeof point.value)
      for (let i = gridData.value.length - 1; award[i] != null; i++) {
        gridData.value.push({
          date: award[i],
          award: awardTime[i]
        })
      }
    }
    return {
      gridData,
      showResult
    }
  },
  data() {
    return {
      start_rotating_degree: 0, //初始旋转角度
      rotate_angle: 0, //将要旋转的角度
      start_rotating_degree_pointer: 0, //指针初始旋转角度
      rotate_angle_pointer: 0, //指针将要旋转的度数
      rotate_transition: 'transform 6s ease-in-out', //初始化选中的过度属性控制
      rotate_transition_pointer: 'transform 12s ease-in-out', //初始化指针过度属性控制
      config: {},
      dialogTableVisible: false,
      confirmVisible: false,
      act1: 'none',
      act2: 'none',
      act3: 'none',
      act4: 'none',
      act5: 'none',
      act6: 'none',
      showAwardPic1: 'none',
      showAwardPic2: 'none',
      showAwardPic3: 'none',
      showAwardPic4: 'none',
      showAwardPic5: 'none',
      changeColor: 'none',
      showResultBoard: 'none',
      resultMessage: '谢谢参与',
      resultTitle: '恭 喜',
      ys: 6480,
      jb: 6480,
      isActive: true
    }
  },
  computed: {
    rotateStyle() {
      return `
        -webkit-transition: transform ${this.config.duration}ms ${this.config.mode};
        transition: transform ${this.config.duration}ms ${this.config.mode};
        -webkit-transform: ${this.rotate_angle};
        
        transform: ${this.rotate_angle};`
    }
  },
  created() {
    // 初始化一些值
    this.angleList = []
    // 是否正在旋转
    this.isRotating = false
    // 基本配置
    this.config = config
  },
  methods: {
    activate() {
      setTimeout(() => (this.showResultBoard = 'block'), 5000)
      setTimeout(() => (this.isActive = true), 5000)
      if (choice == 0) {
        setTimeout(() => (this.act1 = 'block'), 4000)
        this.resultMessage = '二等奖'
        this.resultTitle = '恭 喜'
        this.showAwardPic3 = 'block'
      } else if (choice == 1) {
        setTimeout(() => (this.act2 = 'block'), 4000)
        this.resultMessage = '谢谢参与'
        this.showAwardPic5 = 'block'
        this.resultTitle = '未中奖'
      } else if (choice == 2) {
        setTimeout(() => (this.act3 = 'block'), 4000)
        this.resultMessage = '三等奖'
        this.showAwardPic2 = 'block'
        this.resultTitle = '恭 喜'
      } else if (choice == 3) {
        setTimeout(() => (this.act4 = 'block'), 4000)
        this.resultMessage = '四等奖'
        this.showAwardPic1 = 'block'
        this.resultTitle = '恭 喜'
      } else if (choice == 4) {
        setTimeout(() => (this.act5 = 'block'), 4000)
        this.resultMessage = '谢谢参与'
        this.showAwardPic5 = 'block'
        this.resultTitle = '未中奖'
      } else if (choice == 5) {
        setTimeout(() => (this.act6 = 'block'), 4000)
        this.resultMessage = '一等奖'
        this.showAwardPic4 = 'block'
        this.resultTitle = '恭 喜'
      }
    },
    deactivate() {
      this.showResultBoard = 'none'
      this.showAwardPic1 = 'none'
      this.showAwardPic2 = 'none'
      this.showAwardPic3 = 'none'
      this.showAwardPic4 = 'none'
      this.showAwardPic5 = 'none'
      this.act1 = 'none'
      this.act2 = 'none'
      this.act3 = 'none'
      this.act4 = 'none'
      this.act5 = 'none'
      this.act6 = 'none'
    },
    getRandom() {
      this.isActive = false
      console.log(this.isActive)
      this.ys -= 180
      var today = new Date()
      var storage2 = 495,
        storage3 = 300,
        storage4 = 200,
        storage5 = 5
      var a = 0,
        x = 0.5,
        y = 0.3,
        z = 0.1,
        j = 0.09,
        k = 0.01

      //for (let counter = 0; counter < 4; counter) {
      let answer = Math.random()
      if (answer <= x) {
        let chance = Math.floor(Math.random() * 10) + 1
        if (chance <= 5) {
          a = 4
        } else a = 1
        console.log('谢谢参与')
        this.jb += 300
        //console.log(point.value)
        award.push('谢谢参与')
        awardTime.push(
          today.getFullYear() +
            '.' +
            (today.getMonth() + 1) +
            '.' +
            today.getDate() +
            '.' +
            today.getHours() +
            ':' +
            today.getMinutes()
        )
      } else if (answer <= x + y && storage2 != -1) {
        a = 3
        storage2 -= 1
        console.log('四等奖')
        award.push('四等奖')
        awardTime.push(
          today.getFullYear() +
            '.' +
            (today.getMonth() + 1) +
            '.' +
            today.getDate() +
            '.' +
            today.getHours() +
            ':' +
            today.getMinutes()
        )
      } else if (answer <= x + y + z && storage3 != -1) {
        a = 2
        storage3 -= 1
        console.log('三等奖')
        award.push('三等奖')
        awardTime.push(
          today.getFullYear() +
            '.' +
            (today.getMonth() + 1) +
            '.' +
            today.getDate() +
            '.' +
            today.getHours() +
            ':' +
            today.getMinutes()
        )
      } else if (answer <= x + y + z + j && storage4 != -1) {
        a = 0
        storage4 -= 1
        console.log('二等奖')
        award.push('二等奖')
        awardTime.push(
          today.getFullYear() +
            '.' +
            (today.getMonth() + 1) +
            '.' +
            today.getDate() +
            '.' +
            today.getHours() +
            ':' +
            today.getMinutes()
        )
      } else if (answer <= x + y + z + j + k && storage5 != -1) {
        a = 5
        storage5 -= 1
        console.log('一等奖')
        award.push('一等奖')
        awardTime.push(
          today.getFullYear() +
            '. ' +
            (today.getMonth() + 1) +
            '. ' +
            today.getDate() +
            '.' +
            today.getHours() +
            ':' +
            today.getMinutes()
        )
      }
      if (storage2 == 0) {
        storage2 = -1
        x += y
        y = 0
        //counter += 1
      }
      if (storage3 == 0) {
        storage3 = -1
        x += z
        z = 0
        //counter += 1
      }
      if (storage4 == 0) {
        storage4 = -1
        x += j
        j = 0
        //counter += 1
      }
      if (storage5 == 0) {
        storage5 = -1
        x += k
        k = 0
        //counter += 1
      }
      //}
      choice = a
      this.rotatePointer(a)
    },
    rotatePointer(index) {
      var circle = 6
      const result_angle = [30, 90, 150, 210, 270, 330]
      var rotate_angle =
        this.start_rotating_degree +
        circle * 360 +
        result_angle[index] -
        (this.start_rotating_degree % 360)
      this.start_rotating_degree = rotate_angle
      this.rotate_angle = `rotate(${rotate_angle}deg)`
      console.log(this.rotate_angle)
      this.rotate_angle_pointer = `rotate(' + this.start_rotating_degree_pointer + 360 * circle + 'deg)`
    }
  }
})
</script>
