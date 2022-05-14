<template>
  <div class="flex flex-col items-center p-8">
    <h1 class="text-xl font-medium text-blue-900 mb-8">Create in-video ads in Nuxt.js</h1>
    <div class="relative">
      <video :id="`video-player`" class="w-96 h-96" muted controls></video>

      <div
        :class="showAds ? 'visible' : 'hidden'"
        class="flex justify-between w-96 py-4 px-4 bg-white shadow-lg absolute bottom-0 z-10"
      >
        <video :id="`ad-video-player`" class="w-86 h-96" muted controls></video>
        <button
          class="font-bold text-blue-700 p-2 rounded-full"
          @click="closeShowAds"
        >
          Skip
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      cld: null,
      player: null,
      adPlayer: null,
      showAds: false,
      interval: null,
      timeRun: 0,
      isAdPlaying: false,
    };
  },

  mounted() {
    this.cld = cloudinary.Cloudinary.new({
      cloud_name: process.env.NUXT_ENV_CLOUDINARY_CLOUD_NAME,
      secure: true,
    });

    this.player = this.cld.videoPlayer(`video-player`);
    this.player.source(`/samples/elephants`);
    this.player.play();

    this.adPlayer = this.cld.videoPlayer(`ad-video-player`);
    this.adPlayer.source(`/samples/sea-turtle`);
    this.adPlayer.play();
  },

  methods: {
    closeShowAds() {
      this.showAds = false;
    },
    openShowAds() {
      this.showAds = true;

    },
  },
  created() {
    this.interval = setInterval(() => {
      this.timeRun += 1;
      if (this.timeRun === 5) {
        this.openShowAds();
      }
      if (this.timeRun === 15) {
        this.closeShowAds();
      }
    }, 1000);
  },
};
</script>
