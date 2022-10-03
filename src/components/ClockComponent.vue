<template>
  <div
    class="
      flex flex-col
      items-center
      absolute
      top-1/2
      transform
      -translate-y-1/2
      bg-black/25
      px-4
      py-4
      w-full
    "
  >
    <p class="text-white text-4xl lg:text-7xl">{{ time }}</p>
    <div class="mt-8 space-x-6">
      <button
        type="button"
        class="
          uppercase
          transition
          text-gray-400
          hover:text-white
          text-2xl
          lg:px-6
          py-2
        "
        @click="start"
      >
        start
      </button>

      <button
        type="button"
        class="
          uppercase
          transition
          text-gray-400
          hover:text-white
          text-2xl
          lg:px-6
          py-2
        "
        @click="stop"
      >
        stop
      </button>

      <button
        type="button"
        class="
          uppercase
          transition
          text-gray-400
          hover:text-white
          text-2xl
          lg:px-6
          py-2
        "
        @click="reset"
      >
        reset
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "@vue/reactivity";

const time = ref("00:00:00.000");
const stats = ref({
  timeBegan: null,
  timeStopped: null,
  stoppedDuration: 0,
  started: null,
  running: false,
});

const start = () => {
  if (stats.value.running) return;

  if (stats.value.timeBegan === null) {
    reset();
    stats.value.timeBegan = new Date();
  }

  if (stats.value.timeStopped !== null) {
    stats.value.stoppedDuration += new Date() - stats.value.timeStopped;
  }

  stats.value.started = setInterval(clockRunning, 10);
  stats.value.running = true;
};

const stop = () => {
  stats.value.running = false;
  stats.value.timeStopped = new Date();
  clearInterval(stats.value.started);
};

const reset = () => {
  stats.value.running = false;
  clearInterval(stats.value.started);
  stats.value.stoppedDuration = 0;
  stats.value.timeBegan = null;
  stats.value.timeStopped = null;
  time.value = "00:00:00.000";
};

const clockRunning = () => {
  var currentTime = new Date();
  var timeElapsed = new Date(
    currentTime - stats.value.timeBegan - stats.value.stoppedDuration
  );
  var hour = timeElapsed.getUTCHours();
  var min = timeElapsed.getUTCMinutes();
  var sec = timeElapsed.getUTCSeconds();
  var ms = timeElapsed.getUTCMilliseconds();

  time.value =
    zeroPrefix(hour, 2) +
    ":" +
    zeroPrefix(min, 2) +
    ":" +
    zeroPrefix(sec, 2) +
    "." +
    zeroPrefix(ms, 3);
};

const zeroPrefix = (num, digit) => {
  var zero = "";
  for (var i = 0; i < digit; i++) {
    zero += "0";
  }
  return (zero + num).slice(-digit);
};
</script>