<template>
  <div class="container">
    <!-- if animatedBlock is true animate class will be added from css -->
    <div class="block" :class="{ animate: animatedBlock }"></div>
    <button @click="animateBlock">Animate</button>
  </div>

  <div class="container">
    <transition
      name="para"
      @before-enter="beforeEnter"
      @before-leave="beforeLeave"
      @enter="enter"
      @after-enter="afterEnter"
      @leave="leave"
      @after-leave="afterLeave"
      @enter-cancelled="enterCancelled"
      @leave-cancelled="leaveCancelled"
    >
      <p v-if="paraisvisible">Sometimes visible</p>
    </transition>
    <button @click="togglePara">Toggle Paragraph</button>
  </div>

  <div class="container">
    <transition name="fade-button" mode="in-out">
      <button @click="showUser" v-if="!usersAreVisible">Show users</button>
      <button @click="hideUser" v-else>Hide users</button>
    </transition>
  </div>

  <base-modal @closeDialog="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>

  <div class="container">
    <button @click="showDialog">Show Dialog</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogIsVisible: false,
      animatedBlock: false,
      paraisvisible: false,
      usersAreVisible: false,
      enterInterval: null,
      leaveInterval: null,
    };
  },
  methods: {
    beforeEnter(ele) {
      console.log('before-enter');
      console.log(ele);
      ele.style.opacity = 0;
    },
    beforeLeave(ele) {
      console.log('before-leave');
      console.log(ele);
      ele.style.opacity = 1;
    },
    enter(ele, done) {
      console.log('enter');
      console.log(ele);
      let round = 1;
      this.enterInterval = setInterval(() => {
        ele.style.opacity = round * 0.01;
        round++;
        if (round > 100) {
          clearInterval(this.enterInterval);
          done();
        }
      }, 20);
    },
    afterEnter(ele) {
      console.log('afterEnter');
      console.log(ele);
    },
    leave(ele, done) {
      console.log('leave');
      console.log(ele);
      let round = 1;
      this.leaveInterval = setInterval(() => {
        ele.style.opacity = 1 - round * 0.01;
        round++;
        if (round > 100) {
          clearInterval(this.leaveInterval);
          done();
        }
      }, 20);
    },
    afterLeave(ele) {
      console.log('afterLeave');
      console.log(ele);
    },
    enterCancelled(ele) {
      console.log('enterCancelled');
      console.log(ele);
      clearInterval(this.enterInterval);
    },
    leaveCancelled(ele) {
      console.log('leaveCancelled');
      console.log(ele);
      clearInterval(this.leaveInterval);
    },
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
    animateBlock() {
      this.animatedBlock = true;
    },
    togglePara() {
      this.paraisvisible = !this.paraisvisible;
    },
    showUser() {
      this.usersAreVisible = true;
    },
    hideUser() {
      this.usersAreVisible = false;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
html {
  font-family: sans-serif;
}
body {
  margin: 0;
}

button {
  font: inherit;
  padding: 0.5rem 2rem;
  border: 1px solid #810032;
  border-radius: 30px;
  background-color: #810032;
  color: white;
  cursor: pointer;
}
button:hover,
button:active {
  background-color: #a80b48;
  border-color: #a80b48;
}
.block {
  width: 8rem;
  height: 8rem;
  background-color: #290033;
  margin-bottom: 2rem;
}

.animate {
  animation: slide-fade 0.3s ease-out forwards;
}

.fade-button-enter-from,
.fade-button-enter-to {
  opacity: 0;
}

.fade-button-enter-active {
  transition: opacity 0.3s ease-out;
}

.fade-button-leave-active {
  transition: opacity 0.3s ease-in;
}

.fade-button-enter-to,
.fade-button-enter-from {
  opacity: 1;
}

@keyframes slide-scale {
  0% {
    transform: translateX(0) scale(1);
  }

  70% {
    transform: translateX(-120px) scale(1.1);
  }
  100% {
    transform: translateX(-150px) scale(1);
  }
}
.container {
  max-width: 40rem;
  margin: 2rem auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 2rem;
  border: 2px solid #ccc;
  border-radius: 12px;
}
</style>
