<template>
  <div class="main-content">
    <div class="home-content">
      <Rubberhose 
        :controllers="controllerArray" 
        :locked="['Hose 1::Hip, Hose 2::Hip']"
        :dragggable="[{layer: 'Body'}]"
        :animation-data="animation" />
      <div class="controls">
        <div class="header">
          <svg width="25" height="14" viewBox="0 0 25 14" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect x="11" width="14" height="14" fill="#151515"/>
          <path d="M0.5 6L3.5 9L6.5 6M12.5 5H18.5L22 11M22.5 5L17 11" stroke="#B9B9B9"/>
          <path d="M0.5 6L3.5 9L6.5 6M12.5 13C14 9.5 15 1 17 1C18 1 18.6 1.5 19 2M12.5 5H18.5L22 11M22.5 5L17 11" stroke="#B9B9B9"/>
          </svg>

          <div>
            RubberHose 2
          </div>
          
        </div>
        <div
          class="control-range slider"
          v-for="(controller, i) in realControls"
          :key="i"
        >
        <div> 
          <svg width="24" height="13" viewBox="0 0 24 13" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M1 11L4 8L1 5" stroke="#B9B9B9"/>
          <circle cx="18.5" cy="7.5" r="5" stroke="#B9B9B9"/>
          <rect x="18" width="1" height="3" fill="#B9B9B9"/>
          <path d="M13.707 2.41435L14.4141 1.70725L16 3.5L15.2929 4.20711L13.707 2.41435Z" fill="#B9B9B9"/>
          <rect x="20" width="1" height="3" transform="rotate(90 20 0)" fill="#B9B9B9"/>
          <rect x="14.4141" y="1" width="1" height="2" transform="rotate(45 14.4141 1)" fill="#B9B9B9"/>
          </svg>


          {{ controller.realname }} 
        </div>
          <Input-Scroll
            style="font-size: 16px !important;"
            v-model="controller.value"
            :min="controller.options.min"
            :max="controller.options.max"
            :step="controller.options.interval"
            :suffix="controller.options.suffix"
            flat
          />
          
          <vue-slider v-if="false" v-model="controller.value" v-bind="controller.options" />
        </div>
        <div class="control-range slider">
          <div>
            <svg width="24" height="13" viewBox="0 0 24 13" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="18.5" cy="7.5" r="5" stroke="#B9B9B9"/>
            <rect x="18" width="1" height="3" fill="#B9B9B9"/>
            <path d="M13.707 2.41435L14.4141 1.70725L16 3.5L15.2929 4.20711L13.707 2.41435Z" fill="#B9B9B9"/>
            <rect x="20" width="1" height="3" transform="rotate(90 20 0)" fill="#B9B9B9"/>
            <rect x="14.4141" y="1" width="1" height="2" transform="rotate(45 14.4141 1)" fill="#B9B9B9"/>
            </svg>



            Auto Rotate Start
          </div>
          <Toggle
            label=""
            :state="controls.autoRotateStart.value"
            v-model="controls.autoRotateStart.value"
            size="15px"
          />
        </div>

        <div class="control-range slider">
          <div>
            <svg width="24" height="13" viewBox="0 0 24 13" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="18.5" cy="7.5" r="5" stroke="#B9B9B9"/>
            <rect x="18" width="1" height="3" fill="#B9B9B9"/>
            <path d="M13.707 2.41435L14.4141 1.70725L16 3.5L15.2929 4.20711L13.707 2.41435Z" fill="#B9B9B9"/>
            <rect x="20" width="1" height="3" transform="rotate(90 20 0)" fill="#B9B9B9"/>
            <rect x="14.4141" y="1" width="1" height="2" transform="rotate(45 14.4141 1)" fill="#B9B9B9"/>
            </svg>

            Auto Rotate End
          </div>
          
          <Toggle
            :state="controls.autoRotateEnd.value"
            v-model="controls.autoRotateEnd.value"
            size="15px"
          />
        </div>
        <br>
        <Button 
          class="reset" label="Reset rig" @click="reset" />
      </div>
    </div>
  </div>
</template>

<script>
// import "vue-slider-component/theme/default.css";
import slider from "vue-slider-component";
export default {
  data: () => ({
    rubberhoses: [],
    animation: require("@/assets/walk.json"),
    controls: {
      autoRotateStart: {
        layer: "control",
        name: "rotateStart",
        value: false,
      },
      autoRotateEnd: {
        layer: "control",
        name: "rotateEnd",
        value: true,
      },
      length: {
        layer: "control",
        name: "hoseLength",
        realname: "Hose Length",
        value: 370,
        options: {
          dotSize: 14,
          width: "100%",
          height: 4,
          contained: false,
          direction: "ltr",
          data: null,
          min: 1,
          max: 1600,
          interval: 30,
          clickable: true,
          duration: 0.5,
        },
      },
      radius: {
        layer: "control",
        name: "bendRadius",
        realname: "Bend Radius",
        value: 60,
        options: {
          dotSize: 14,
          width: "100%",
          height: 4,
          contained: false,
          direction: "ltr",
          data: null,
          min: 0,
          max: 100,
          interval: 4,
          clickable: true,
          duration: 0.5,
          suffix: '%',
        },
      },
      realism: {
        layer: "control",
        name: "realism",
        realname: "Realism",
        value: 0,
        options: {
          dotSize: 14,
          width: "100%",
          height: 4,
          contained: false,
          direction: "ltr",
          data: null,
          min: 0,
          max: 100,
          interval: 4,
          clickable: true,
          duration: 0.5,
          suffix: '%',
        },
      },
      direction: {
        layer: "control",
        name: "bendDirection",
        realname: "Bend Direction",
        value: 100,
        options: {
          dotSize: 14,
          width: "100%",
          height: 4,
          contained: false,
          direction: "ltr",
          data: null,
          min: -100,
          max: 100,
          interval: 7,
          clickable: true,
          duration: 0.5,
        },
      },
    },
  }),
  components: {
    Rubberhose: require("rubberhose-lottie").default,
    "vue-slider": slider,
  },
  computed: {
    // The prop expects an Array, but I'd prefer to keep them as Objects
    // in data above. So we just convert the parent data objects to an Array:
    controllerArray() {
      let temp = [];
      Object.keys(this.controls).forEach((key) => {
        if (!/options/.test(key)) temp.push(this.controls[key]);
      });
      return temp;
    },
    realControls() {
      let temp = [];
      Object.keys(this.controls).forEach((key) => {
        if (!/autoRotate/.test(key)) temp.push(this.controls[key]);
      });
      return temp;
    },
  },
  methods: {
    assignRubberhoses(val) {
      console.log(val);
    },
    reset() {
      location.reload()
    },
  },
};
</script>

<style>
:root {
  --blue: #569FE9;
  --text: #939393;
  --bg: #262626;
}
.main-content {
  display: flex;
  justify-content: center;
  width: 100%;
  align-items: flex-start;
  background: var(--bg);
}
.rubberhose-animation {
  margin-bottom: -6px;
}
.home-content {
  width: 800px;
}

.anim-main {
  fill: var(--color-default);
}
.reset {
  font-size: 13px;
  border: 1px solid #555 !important;
  margin: 0 12px;
}

.controls {
  width: 240px;
  min-height: 40px;
  background: var(--bg);
  color: var(--text);
  user-select: none;
  padding-bottom: 16px;
  /* border: 1px solid var(--blue); */
  box-sizing: border-box;
}
.controls .header {
  float: left;
  margin: 2px;
  background: #1E1E1E;
  width: -webkit-fill-available;
  font-size: 12px;
  border-top: 2px solid #000;
  padding-top: 2px;
}
.header>* {
  float: left;
  margin-left: 6px;
  margin-right: -1px;
  text-align: left;
}
.control-range {
  display: flex;
  justify-content: center;
  flex-wrap: nowrap;
  width: 100%;
  box-sizing: border-box;
  padding: 0px 8px;
  align-items: center;
  font-size: 12px !important;
  border-bottom: 1px solid #1a1a1a;
  height: 20px;
}

.control-range.slider {
  display: grid;
  grid-template-columns: 1.5fr 0.5fr;
  /* width: calc(100% - 40px);
  justify-items: center; */
}

.control-range > * {
  /* margin: 0px 30px; */
  text-align: left;
  margin-top: -1px;
}

.control-range svg {
  margin: 0 2px -2px 0px;
  /* margin-right: 4px; */
}
.toggle-item {
  margin-bottom: -3px;
}
.toggle-contents .label {
  user-select: none;
  cursor: default;
}
.input-scroll-container {
  height: 18px;
}
.input-scroll-value {
  font-size: 12px !important;
  color: var(--blue) !important;
}
.input-scroll-label {
  font-size: 12px !important;
}
.input-scroll-wrapper {
  margin-left: -4px;
  margin-top: -5px;
}
.input-scroll-field span {
  font-size: 11px;
  margin-left: -4px;
}

.rh-control {
  fill: #00aaff;
  stroke: #00aaff;
}
.rh-main {
  stroke: #ffaa00;
}
.rh-bg {
  fill: rgba(1, 1, 1, 0.02);
}
.home-content {
  width: 100%;
}
.rubberhose-container {
  float: right;
  width: calc(100% - 240px);
  background: white;
}
@media only screen and (max-width: 600px) {
  .control-range.toggles {
    flex-direction: column;
  }
  .control-range.toggles > * {
    margin: 4px 0px;
  }
  .controls {
    position: relative;
    width: 100vw;
    height: auto;
    top: 0;
    left: 0;
  }
  .rubberhose-container {
    float: none;
    width: auto;
  }
}
</style>