<template>
  <div id="app">
    <div class="box" >
      <img src="./assets/disk.jpg" class="pan"  alt="转盘" ref="pan" :style="style">
      <img src="./assets/start.png" class="zhen" alt="指针" @click="rotatingStart">
    </div>
  </div>
</template>

<script>
  import {Dialog} from 'vant'
  export default {
  name: 'app',
  data(){
    return{
      style:{},
      prizeList : [//每个等级奖励所带的信息
// 一等奖：华为 Mate 30 Pro（8G+256G）手机一部，共 70 部，中奖率
//     0.04%；
// 二等奖：水井坊 100ml 井台12小酒礼盒一个，共 41000 个,中奖率
//     25.00%；
// 三等奖：微信现金红包一个（金额为 188 元 / 1.88 元 / 0.88 元其中之
//     一），随机发放：
//     188 元，共 200 个，中奖率 0.12 %
//     1.88 元，共 85506 个，中奖率 52.14 %
//     0.88 元，共 37224 个，中奖率 22.70 %
//     本活动综合中奖率 100%。
        {
          id: 1,
          content:'一等奖',
          name: '华为 Mate 30 Pro（8G+256G）',
        },
        {
          id: 2,
          content:'二等奖',
          name: '水井坊 100ml 井台12小酒礼盒一个',
        }, {
          id: 3,
          content:'微信红包',
          name: '188 元',
        }, {
          id: 4,
          content:'微信红包',
          name: '1.88 元',
        },{
          id: 5,
          content:'微信红包',
          name: '0.88 元',
        }
      ],
      prizeNum:0,
      finalAngle:0
    }
  },
  mounted(){

  },
  methods:{
    /**
     * @return {number}
     */
    LimitRandom(min,max){//两个角度之间产生一个随机角度
      return Math.floor(Math.random()*(max-min+1)+min);
    },
    getOne(num1,num2){//从这两个数中随机返回其中一个
      const num = Math.random();
      if(num<0.5)return num1;
      else return num2;
    },
    rotatingStart(){
      const randomNum = Math.random().toFixed(4);//去个随机数/ 0.0004=0.04%
      if(randomNum<0.0004){//设置奖品的概率 一等
        this.prizeNum=1;
      }else if(randomNum<0.2504){//二等
        this.prizeNum=2;
      }else if(randomNum<0.2516){//三等1
        this.prizeNum=3;
      }else if(randomNum<0.773){//三等2
        this.prizeNum=4;
      }else {//三等3
        this.prizeNum=5;
      }
      switch (this.prizeNum) {
        case 1:this.finalAngle=this.LimitRandom(0,30);break;
        case 2:this.finalAngle=this.LimitRandom(240,270);break;
        case 3:this.finalAngle=this.LimitRandom(90,120);break;
        case 4:
          const a1 = this.LimitRandom(60,90);
          const a2 = this.LimitRandom(180,210);
          const a3 = this.LimitRandom(300,330);
          const a4 = this.getOne(a1,a2);
          this.finalAngle=this.getOne(a3,a4);
          break;
        case 5:
          const b1 = this.LimitRandom(30,60);
          const b2 = this.LimitRandom(90,120);
          const b3 = this.LimitRandom(150,180);
          const b4 = this.LimitRandom(210,240);
          const b5 = this.LimitRandom(270,300);
          const b6 = this.LimitRandom(330,360);
          const b7 = this.getOne(b1,b2);
          const b8 = this.getOne(b3,b4);
          const b9 = this.getOne(b5,b6);
          const b10 = this.getOne(b7,b8);
          this.finalAngle=this.getOne(b9,b10);
          break;
      }
      this.style={
        transform:  `rotate(${-(1080+this.finalAngle)}deg)`,
        transition: "all 3s"
      };
      console.log(this.prizeNum,this.finalAngle);
      setTimeout(()=>{
        const text = this.prizeList.filter(item=>this.prizeNum===item.id)[0];//获取该奖的文本信息
        Dialog.alert({
          title: text.content,
          message: text.name
        }).then(() => {
          this.style={
            transform:  `rotate(0deg)`,
            transition: "all 0s"
          };
        });

      },3500)
    },

  },
}
</script>

<style>

.box{
  animation: all 3s;
  height: 500px;
  position: relative;
  background: #ffffff;
}
  .zhen{
    width: 90px;
    height: 180px;
    position: absolute;
    margin-top: -90px;
    margin-left: -45px;
    top: 50%;
    left: 50%;
  }
  .pan{
    transform: rotate(0deg);
    position: absolute;
    width: 400px;
    height: 400px;
    margin-top: -200px;
    margin-left: -200px;
    top: 50%;
    left: 50%;
  }
</style>
