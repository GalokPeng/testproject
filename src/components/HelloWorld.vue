<!-- YourVideoPlayer.vue -->

<template>
  <div>
    <div>
      <label for="videoUrl">Video URL:</label>
      <input v-model="currentVideoUrl" id="videoUrl" type="text" />
      <button @click="changeVideo">Change Video</button>
    </div>
    <video ref="videoPlayer" class="video-js vjs-default-skin" controls></video>
  </div>
</template>

<script setup lang="ts">
import videojs from 'video.js';
import { ref, onBeforeUnmount, onMounted } from 'vue';
import 'video.js/dist/video-js.css';

// Component setup
const videoPlayer:any = ref<HTMLVideoElement | null>(null);
const currentVideoUrl = ref<string>('https://vod3.jegms.com/20230912/j0GRBhT4/index.m3u8');

// Initialize video player when the component is mounted
onMounted(() => {

  // Ensure to destroy the player to prevent memory leaks when the component is unmounted
  onBeforeUnmount(() => {
    const player:any = videojs(videoPlayer.value);
    if (player) {
      player.dispose();
    }
  });

  // Add event listeners for keyboard shortcuts
  window.addEventListener('keydown', handleKeyDown);
});

// Method to initialize video player
// const initializeVideoPlayer = () => {
//   if (videoPlayer.value) {
//     // Initialize the video player
//     const player = videojs(videoPlayer.value, {
//       controls: true,
//       autoplay: false,
//       fluid: true,
//       sources: [
//         {
//           src: currentVideoUrl.value,
//           type: 'application/x-mpegURL',
//         },
//       ],
//     });
//   }
// };

// Method to update video source
const updateVideoSource = () => {
  if (videoPlayer.value) {
    const player = videojs(videoPlayer.value);
    player.src({
      src: currentVideoUrl.value,
      type: 'application/x-mpegURL',
    });
    player.load(); // Load the new source
    player.play(); // Start playing
  }
};

// Method to change the video based on user input
const changeVideo = () => {
  updateVideoSource();
};

// Method to handle keyboard shortcuts
const handleKeyDown = (event: KeyboardEvent) => {
  if (videoPlayer.value) {
    const player:any = videojs(videoPlayer.value);
    switch (event.key) {
      case ' ':
        player.paused() ? player.play() : player.pause(); // Spacebar to play/pause
        break;
      case 'ArrowRight':
        player.currentTime(player.currentTime() + 10); // Right arrow key to fast forward 10 seconds
        break;
      case 'ArrowLeft':
        player.currentTime(player.currentTime() - 10); // Left arrow key to rewind 10 seconds
        break;
    }
  }
};
</script>

<style scoped>
/* Customize styles as needed */
</style>
