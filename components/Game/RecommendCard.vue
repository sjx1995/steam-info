<!--
 * @Description: 3d卡片
 * @Author: Sunly
 * @Date: 2023-07-14 05:17:04
-->
<script lang="ts" setup>
defineProps<{
  name: string;
  price: string;
  windows_available: boolean;
  mac_available: boolean;
  linux_available: boolean;
  imgUrl: string;
}>();

const moveContext = reactive({
  isEnter: false,
  centerX: 0,
  centerY: 0,
  distance: 0,
});
const transformValue = computed(() =>
  moveContext.isEnter
    ? `
      scale3d(1.2, 1.2, 1.2)
      rotate3d(
        ${moveContext.centerY / 100},
        ${-moveContext.centerX / 100},
        0,
        ${Math.log(moveContext.distance) * 2}deg
      )`
    : "scale3d(1, 1, 1) rotate3d(0, 0, 0, 0deg)"
);
const rotateToMouse = (e: MouseEvent) => {
  if (!moveContext.isEnter) return;
  const { offsetX, offsetY } = e;
  const wrapperWidth = 308;
  const wrapperHeight = 176;
  moveContext.centerX = offsetX - wrapperWidth / 2;
  moveContext.centerY = offsetY - wrapperHeight / 2;
  moveContext.distance =
    Math.sqrt(moveContext.centerX ** 2 + moveContext.centerY ** 2) * 1.6;
};
</script>
<template>
  <div class="game-card-container">
    <div
      class="game-card-wrapper"
      :style="{ transform: transformValue }"
      @mousemove="rotateToMouse"
      @mouseenter="() => (moveContext.isEnter = true)"
      @mouseleave="() => (moveContext.isEnter = false)"
    >
      <div class="game-card-shadow"></div>
      <div class="game-card">
        <img :src="imgUrl" />
        <div class="game-card-info">
          <div class="title">
            {{ name }}
          </div>
          <div class="info">
            {{ price }}
            <span>
              <span
                v-if="windows_available"
                class="mdi mdi-microsoft-windows"
              ></span>
              <span v-if="mac_available" class="mdi mdi-apple"></span>
              <span v-if="linux_available" class="mdi mdi-linux"></span>
            </span>
          </div>
        </div>
        <!-- <div class="loading-container" v-if="!imgLoaded">
          <LoadingComponent />
        </div> -->
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.game-card-container {
  width: 400px;
  height: 240px;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  perspective: 800px;
  .game-card-wrapper {
    position: relative;
    width: 308px;
    height: 176px;
    transition: all 0.2s ease-out;
    border-radius: 12px;
    .game-card-shadow {
      position: absolute;
      top: 5%;
      left: 5%;
      width: 90%;
      height: 90%;
      transition: all 0.2s ease-out;
      box-shadow: 0 0 transparent;
    }
    &:hover {
      .game-card-shadow {
        box-shadow: 0 20px 100px rgba(255, 0, 242, 0.5),
          0 16px 40px rgba(255, 0, 242, 0.5);
      }
      .game-card-info {
        bottom: 0px !important;
        .title {
          margin-bottom: 0px !important;
        }
      }
    }
    .game-card {
      width: 100%;
      height: 100%;
      overflow: hidden;
      transform-style: preserve-3d;
      border-radius: 8px;
      box-shadow: 0 2px 12px -3px rgba(255, 0, 242, 0.5);
      .loading-container {
        width: 100%;
        height: 100%;
        background: #222;
        display: flex;
        justify-content: center;
        align-items: center;
      }
      img {
        width: 100%;
        height: 100%;
      }
      .game-card-info {
        position: absolute;
        bottom: -22px;
        height: 100px;
        width: 100%;
        padding: 0 18px;
        user-select: none;
        background: linear-gradient(
          0deg,
          rgba(0, 0, 0, 0.65) 0%,
          rgba(0, 0, 0, 0.45) 60%,
          transparent 100%
        );
        transition: all 0.2s ease-out;
        .title {
          font-size: 18px;
          margin-top: 40px;
          margin-bottom: 12px;
          overflow: hidden;
          text-wrap: nowrap;
          text-overflow: ellipsis;
          transition: all 0.2s ease-out;
        }
        .info {
          font-size: 14px;
          display: flex;
          justify-content: space-between;
          line-height: 24px;
          .mdi {
            font-size: 18px;
            margin-left: 8px;
          }
        }
      }
    }
  }
}
</style>
