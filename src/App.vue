<template>
  <div class="viewer">
    <vc-viewer @ready="ready">
      <vc-primitive-tileset
        :url="url"
        @readyPromise="readyPromise"
        @initialTilesLoaded="initialTilesLoaded"
        @allTilesLoaded="allTilesLoaded"
        @loadProgress="loadProgress"
        @tileFailed="tileFailed"
        @tileUnload="tileUnload"
        @tileVisible="tileVisible"
        @click="clicked"
      >
      </vc-primitive-tileset>
    </vc-viewer>
    <!-- <div id="video">
      <vue-aliplayer-v2
        :source="source"
        ref="VueAliplayerV2"
        :options="options"
      ></vue-aliplayer-v2>
      <button @click="play()">开始播放</button>
    </div> -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      url:
        "http://192.168.40.25:9000/model/a6e9e5405eda11eb8838394d2b5e012b/tileset.json",
      options: {
        // source:'//player.alicdn.com/video/aliyunmedia.mp4',
        isLive: true, //切换为直播流的时候必填
        format: "m3u8", //切换为直播流的时候必填
      },
      source:
        "ima.hcmis.cn/live/34020000001310000001.m3u8?auth_key=1611567842-0-0-a8fc6ef23140961fdae9dd424be700a4",
      // source: "//ivi.bupt.edu.cn/hls/cctv1.m3u8",
    };
  },
  methods: {
    clicked(a) {
      console.log(a);
    },
    ready(cesiumInstance) {
      this.cesiumInstance = cesiumInstance;
      // const { Cesium, viewer } = cesiumInstance;
    },
    readyPromise(tileset) {
      const { Cesium, viewer } = this.cesiumInstance;
      viewer.zoomTo(
        tileset,
        new Cesium.HeadingPitchRange(
          0.0,
          -0.5,
          tileset.boundingSphere.radius * 0.25
        )
      );
    },
    allTilesLoaded() {
      console.log("All tiles are loaded");
    },
    initialTilesLoaded() {
      console.log("Initial tiles are loaded");
    },
    loadProgress(numberOfPendingRequests, numberOfTilesProcessing) {
      if (numberOfPendingRequests === 0 && numberOfTilesProcessing === 0) {
        console.log("Stopped loading");
        return;
      }

      console.log(
        "Loading: requests: " +
          numberOfPendingRequests +
          ", processing: " +
          numberOfTilesProcessing
      );
    },
    tileFailed(error) {
      console.log("An error occurred loading tile: " + error.url);
      console.log("Error: " + error.message);
    },
    play() {
      alert("我要播放");
      this.$refs.VueAliplayerV2.play();
    },
  },
  mounted() {},
};
</script>

<style scoped>
body,
* {
  padding: 0px;
  margin: 0px;
}
.viewer {
  width: 100vw;
  height: 100vh;
  padding: 0px;
  margin: 0px;
}
#video {
  position: absolute;
  top: 20px;
  left: 20px;
  z-index: 999;
  width: 480px;
  border-radius: 10px;
}
</style>