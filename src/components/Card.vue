<template>
  <div class="card" ref="root" @mousemove="moving" @mouseleave="style.transform = ''">
      <div class="card-image" :style="style">
        <div ref="glow" class="glow"></div>
      </div>
  </div>
</template>

<script setup>
  import imgUrl from '../assets/image.png'
  import {onMounted, ref} from "vue";
  const style = ref({transform: ''});
  const root = ref(null);
  const glow = ref(null);

  onMounted(() => {
      style.value.backgroundImage = `url(${imgUrl})`
      style.value.backgroundPosition = 'center';
      style.value.height = "100%";
      style.value.width = "100%";
      style.value.transitionDuration = '150ms';
  })

  function moving(e) {
      let bound = root.value.getBoundingClientRect();
      const mouseX = e.clientX;
      const mouseY = e.clientY;
      const leftX = mouseX - bound.x;
      const topY = mouseY - bound.y;
      const center = {
          x: leftX - bound.width / 2,
          y: topY - bound.height / 2
      }

      style.value.transform = `rotateX(${center.y/8}deg) rotateY(${-center.x/8}deg)`;

      glow.value.style.backgroundImage = `radial-gradient(
      circle at
      ${center.x * 2 + bound.width/2}px
      ${center.y * 2 + bound.height/2}px,
      #ffffff55,
      #0000000f
    )`
  }
</script>

<style scoped>
.card {
    width: 500px;
    height: 300px;
    position: relative;
    transition-duration: 150ms;
    overflow: hidden;
}

.card:hover {
    transform: scale(110%);
}

.card-image {
    border-radius: 10px;
    position: relative;
    background-size: cover;
    background-repeat: no-repeat;
    overflow: hidden;
}

.card-image:hover {
    box-shadow: 0 5px 20px 5px #00000044;
}

.glow {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;

    background-image: radial-gradient(circle at 50% -20%, #ffffff22, #0000000f);
}

</style>