<template>
  <Scene>
    <BackgroundParticules
      class="absolute top-0 left-0 right-0 bottom-0 w-full h-full"
    />
    <div
      class="relative flex-col flex gap-2 bg-slate-600/50 rounded-2xl p-7 shadow"
    >
      <h1 class="text-center font-bold text-xl uppercase mb-6">
        {{ settings.name }}
      </h1>
      <div v-if="!started" class="relative play-container" @click="start">
        <img :src="cover" :width="width" :height="width" />
        <div
          class="absolute border border-slate-100 rounded-full top-1/2 left-1/2 -translate-1/2 p-3 bg-black/20 play-button"
          style="line-height: 0"
        >
          <UIcon
            name="i-line-md-pause-to-play-filled-transition"
            class="size-5"
          />
        </div>
      </div>
      <div v-else class="relative">
        <svg
          :width="width"
          :height="width"
          :viewBox="'0 0 ' + width + ' ' + width"
          xmlns="http://www.w3.org/2000/svg"
        >
          <defs>
            <pattern
              id="image-pattern"
              patternUnits="userSpaceOnUse"
              :width="width"
              :height="width"
            >
              <image
                :href="settings.bg"
                x="0"
                y="0"
                :width="width"
                :height="width"
                preserveAspectRatio="xMidYMid slice"
              />
              <image
                :href="settings.gif"
                x="25"
                y="60"
                :width="width - 60"
                :height="width - 60"
                preserveAspectRatio="xMidYMid slice"
              />
            </pattern>
          </defs>
          <rect
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
          />
        </svg>
      </div>
      <div class="mt-6">
        <div class="relative bg-slate-300 rounded-full w-full h-2">
          <div
            class="top-0 bottom-0 absolute left-0 w-0 bg-fuchsia-600 rounded-full transition-all ease-linear"
            :style="'transition-duration: ' + settings.time + 's'"
            :class="started ? 'w-full' : ''"
          ></div>
          <div
            class="top-1/2 absolute w-4 h-4 left-0 bg-fuchsia-600 rounded-full border border-slate-50 -translate-y-1/2 -translate-x-1/2 transition-all ease-linear volume"
            :style="'transition-duration: ' + settings.time + 's'"
            :class="started ? 'left-full' : ''"
          ></div>
        </div>
        <div class="flex gap-2 justify-center items-center mt-5">
          <UIcon name="i-line-md-chevron-left-circle-twotone" size="30" />
          <UIcon
            :name="
              started
                ? 'i-line-md-play-filled-to-pause-transition'
                : 'i-line-md-pause-to-play-filled-transition'
            "
            size="30"
          />
          <UIcon name="i-line-md-chevron-right-circle-twotone" size="30" />
        </div>
      </div>
    </div>
  </Scene>
</template>
<script setup>
import bg from "~/assets/images/1.jpg";
import cover from "~/assets/images/tt.png";
import gif from "~/assets/images/tt2.gif";
const settings = reactive({
  name: "Caliente",
  bg,
  cover,
  gif,
  time: 20,
});

const width = 300;
const size = 20;
const nb = width / size;
const space = 5;
const items = ref(
  [...Array(nb)].map((_, i) => {
    return {
      x: i * size + space * i,
      y: width / 2 - 180 / 2,
      width: size,
      height: 180,
    };
  })
);
const started = ref(false);

function update() {
  items.value.forEach((item) => {
    const min = 40;
    const height =
      width * ((Math.floor(Math.random() * (100 - min + 1)) + min) / 100);
    item.height = height;
    item.y = width / 2 - height / 2;
  });
}

function start() {
  started.value = true;
  setInterval(() => {
    update();
  }, 200);
}

onMounted(() => {
  update();
});
</script>
<style lang="scss" scoped>
.play-container {
  @apply bg-black;
}
.play-container:hover {
  .play-button {
    @apply bg-black;
  }
}
</style>
