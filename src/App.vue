<template>
  <div class="container">
    <div class="block" :class="{ animate: animatedBlock }"></div>
    <button @click="animateBlock">Animate</button>
  </div>
  <base-modal @close="hideDialog" :open="dialogIsVisible">
    <p>This is a test dialog!</p>
    <button @click="hideDialog">Close it!</button>
  </base-modal>

  <div class="container">
    <div>
      <transition name="fade-button" mode="out-in">
        <button @click="showUser" v-if="!userIsVisible">Show Users</button>
        <button @click="hideUser" v-else-if="userIsVisible">Hide Users</button>
      </transition>
      <p v-if="userIsVisible">user Anton</p>
    </div>
  </div>

  <div class="container">
    <transition
      name="paragraph"
      @before-enter="beforeEnter"
      @enter="enter"
      @after-enter="afterEnter"
      @before-leave="beforeLeave"
      @leave="leave"
      @after-leave="afterLeave"
      @enter-cancelled="enterCancelled"
      @leave-cancelled="leaveCancelled"
    >
      <p v-if="paragraphIsVisible">This is only sometimes visible..</p>
    </transition>
    <button @click="toggleParagraph">Toggle paragraph</button>
  </div>
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
      paragraphIsVisible: false,
      userIsVisible: false,
      enterInterval: null,
      leaveInterval: null,
    };
  },
  methods: {
    showDialog() {
      this.dialogIsVisible = true;
    },
    hideDialog() {
      this.dialogIsVisible = false;
    },
    animateBlock() {
      this.animatedBlock = true;
    },
    toggleParagraph() {
      this.paragraphIsVisible = !this.paragraphIsVisible;
    },
    showUser() {
      this.userIsVisible = true;
    },
    hideUser() {
      this.userIsVisible = false;
    },
    beforeEnter(el) {
      console.log('before-enter', el);
      el.style.opacity = 0;
    },
    enter(el, done) {
      console.log('enter', el);
      let round = 1;
      this.enterInterval = setInterval(() => {
        el.style.opacity = round * 0.01;
        round++;
        if (round > 100) {
          clearInterval(this.enterInterval);
          done();
        }
      }, 20);
    },
    afterEnter(el) {
      console.log('Affter enter..', el);
    },
    beforeLeave(el) {
      console.log('before-leave',el);
      el.style.opacity = 1;
    },

    leave(el,done) {
      console.log('leave...', el);
      let round = 1;
      this.leaveInterval = setInterval(() => {
        el.style.opacity = 1 - round * 0.01;
        round++;
        if (round > 100) {
          clearInterval(this.leaveInterval);
          done();
        }
      }, 20);
    },
    afterLeave() {
      console.log('Affter leave ..');
    },
    enterCancelled(el) {
        console.log('enterCancelled', el);
        clearInterval(this.clearInterval);
    },
    leaveCancelled(el) {
      console.log('leaveCancelled',el);
      clearInterval(this.leaveInterval);
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
  outline: none;
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
  /* transition: all 0.3s; */
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
.animate {
  /* transform: translateX(-50px); */
  animation: slide-scale 0.3s ease-out forwards;
}
.fade-button-enter-from,
.fade-button-leave-to {
  opacity: 0;
}
.fade-button-enter-active {
  transition: opacity 0.5s ease-out;
}
.fade-button-leave-active {
  transition: opacity 0.5s ease-in;
}
.fade-button-enter-to,
.fade-button-leave-from {
  opacity: 1;
}

.v-enter-from {
  /* opacity: 0;
  transform: translateY(-50px); */
}
.v-enter-active {
  /* transition: all .5s ease-out; */
  /* animation: slide-scale .3s ease-out;    */
}
/* .paragraph-enter-active { */
/* transition: all .5s ease-out; */
/* animation: slide-scale 2s ease-out; */
/* } */
.v-enter-to {
  /* opacity: 1;
  transform: translateY(0); */
}
.v-leave-from {
  /* opacity: 1;
  transform: translateY(0); */
}
.v-leave-active {
  /* transition: all .5s ease-in; */
  /* animation: slide-scale .3s ease-in;    */
}
/* .paragraph-leave-active { */
/* transition: all .5s ease-out; */
/* animation: slide-scale 0.3s ease-in; */
/* } */
.v-leave-to {
  /* opacity: 0;
  transform: translateY(-40px); */
}
@keyframes slide-scale {
  0% {
    transform: translateX(0) scale(1);
  }
  50% {
    transform: translateX(-25px) scale(1.1);
  }
  100% {
    transform: translateX(-50px) scale(1);
  }
}
</style>