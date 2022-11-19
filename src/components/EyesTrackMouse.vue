<template>
  <div class="container">
    <div class="eye eye-left">
      <div class="eye-inner"></div>
    </div>
    <div class="eye eye-right">
      <div class="eye-inner">
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted } from 'vue';

export default defineComponent({
  setup() {
    const calculateRotationByMousePosition = ({event, mouseX, mouseY} : {event: MouseEvent, mouseX: number, mouseY: number}) => {
      const { pageX, pageY } = event;

      const radianDegrees =  Math.atan2(pageX - mouseX, pageY - mouseY);
      return radianDegrees * (180 / Math.PI) * -1 + 180;
    }
    onMounted(() => {
      const container = document.querySelector('.container');
      if (!container) {
        return;
      }

      container.addEventListener('mousemove', (event => {
        const eyes = document.querySelectorAll('.eye');
        eyes.forEach((eye) => {
          const mouseX = eye.getBoundingClientRect()[eye.classList.contains( 'eye-left' ) ? 'left' : 'right']
          const mouseY = eye.getBoundingClientRect().top;
          const rotationDegrees = calculateRotationByMousePosition({ event: event as MouseEvent, mouseX, mouseY });
          (eye as HTMLElement).style.transform =  `rotate(${rotationDegrees}deg)`;
        })
      }))
    })
  }
})
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
}
.eye {
  display: flex;
  width: 48px;
  height: 48px;
  bottom: 41px;
  background: #fff;
  border-radius: 50%;
  &-left {
    left: 26px;
  }
  &-right {
    right: 26px;
  }
}
.eye {
  &-inner {
    position: relative;
    display: inline-block;
    border-radius: 50%;
    width: 40px;
    height: 40px;
    background-color: black; border-radius: 50%; margin-left: 4px; margin-top: 4px;
    &:after {
      position: absolute;
      top: 2px;
      left: 10px;
      width: 20px;
      height: 20px;
      background: white;
      border-radius: 50%;
      content: '';
    }
  }
}

</style>