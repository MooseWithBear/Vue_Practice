<template>
  <div>
    <div id="screen" @click="onClickScreen" :class="state">{{ message }}</div>
    <div>
      <div>평균시간: {{ average }}ms</div>
      <button @click="onReset">리셋</button>
    </div>
  </div>
</template>

<script>
let startTime = 0;
let endTime = 0;
let timeout = null;

export default {
  data() {
    return {
      result:[],
      state:'waiting',
      message:'클릭해서 시작하세요.'
    };
  },
  computed: {
    average() {
      return this.result.reduce((a,c) => a + c, 0) / this.result.length || 0;
    }
  },
  methods: {
    onReset() {
      this.result = [];
    },
    onClickScreen() {
      if(this.state === 'waiting') {
        this.state = 'ready';
        this.message = '초록색이 되면 클릭하세요!'
        setTimeout(() => {
          this.state = 'now'
          this.message = '지금클릭!'
          startTime = new Date();
        }, Math.floor(Math.random() * 1000) + 2000); //2~3초 후에
      } else if (this.state === 'ready') {
        clearTimeout(timeout);
        this.state = 'waiting';
        this.message = '너무 빨리 클릭하셨습니다 다시시작하세요!'
      } else if (this.state === 'now') {
        endTime = new Date();
        this.state = 'waiting';
        this.message = '클릭해서 시작하세요'
        this.result.push(endTime-startTime)
      }
    }
  },
};
</script>

<!-- scoped를 붙여놓으면 css는 해당 컴포넌트 내에서만 한정 작동한다. -->
<style scoped> 
#screen {
  width: 300px;
  height: 200px;
  text-align: center;
  user-select: none;
}
#screen.waiting {
  background-color: aqua;
}
#screen.ready {
  background-color: red;
  color: white;
}
#screen.now {
  background-color: greenyellow;
}
</style>
