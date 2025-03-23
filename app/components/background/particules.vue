<template>
  <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
    <defs>
      <pattern
        id="linear-gradient"
        patternUnits="userSpaceOnUse"
        width="100"
        height="100"
      >
        <linearGradient
          :x1="bgPos.x + '%'"
          :y1="bgPos.y + '%'"
          :x2="bgPos.x2 + '%'"
          :y2="bgPos.y2 + '%'"
          id="cc"
        >
          <stop offset="0%" style="stop-color: #ee208b; stop-opacity: 0.5" />
          <stop offset="100%" style="stop-color: #5ee0fe; stop-opacity: 0.5" />
        </linearGradient>
        <rect
          x="0"
          y="0"
          width="100%"
          height="100%"
          fill="url(#cc)"
          class="transition-all duration-600 ease-in"
        ></rect>
      </pattern>

      <filter id="pattern-shadow">
        <feGaussianBlur stdDeviation="2" />
      </filter>
    </defs>

    <rect
      v-for="item in items"
      :key="item"
      :x="item.x"
      :y="item.y"
      rx="10"
      ry="10"
      :width="item.w"
      height="100%"
      fill="url(#linear-gradient)"
      filter="url(#pattern-shadow)"
      class="transition-all duration-500 ease-linear light"
    ></rect>

    <!-- <rect
      v-for="item in items"
      :key="item.x"
      :x="item.x"
      :y="item.y"
      :width="item.width"
      :height="item.height"
      rx="10"
      ry="10"
      fill="url(#image-pattern)"
      class="transition-all duration-200 ease-in"
    /> -->
  </svg>
</template>
<script setup>
const bgPos = reactive({ x: 0, y: 0, x2: 100, y2: 0 });
const items = reactive([{ x: 0, y: 0, w: 3 }]);
onMounted(() => {
  setInterval(() => {
    bgPos.x += Math.random() * 10 - 5;
    bgPos.y += Math.random() * 10 - 5;
    bgPos.x2 += Math.random() * 10 - 5;
    bgPos.y2 += Math.random() * 10 - 5;
  }, 100);
  setInterval(() => {
    const i = items.push({
      x: Math.random() * 100,
      y: 200,
      w: Math.floor(Math.random() * (10 - 3 + 1)) + 3,
    });
    setTimeout(() => items.shift(), 5000);
  }, 1000);
  setInterval(() => {
    items.forEach((e) => {
      e.y -= 50;
    });
  }, 500);
});
</script>
