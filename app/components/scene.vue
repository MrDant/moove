<template>
  <div
    class="relative w-screen h-screen flex flex-col items-center justify-center gap-1 bg-slate-900 text-white"
  >
    <div id="capture-area">
      <slot />
    </div>

    <img id="preview" />
  </div>
</template>

<script setup>
import domtoimage from "dom-to-image";

async function startRecording() {
  const element = document.getElementById("capture-area");
  domtoimage.toPng(element).then(function (dataUrl) {
    document.getElementById("preview").src = dataUrl;
  });
}

function stopRecording() {
  document.getElementById("preview").src = "";
}
</script>
