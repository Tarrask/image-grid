<template>
  <div>
    <div class="controls-container p-2 d-flex align-items-start">
      <button class="btn btn-outline-primary mr-2" @click="controlsVisible = !controlsVisible">III</button>
      <div :class="{ hidden: !controlsVisible }" class="controls">
        <div class="form-group">
          <div class="custom-file">
            <input type="file" ref="fileInput" class="custom-file-input" id="customFile" @change="updateImage">
            <label class="custom-file-label" for="customFile">{{ imageName }}</label>
          </div>
        </div>
        <div class="form-group">
          <label for="gridCount">Grid size</label>
          <input id="gridCount" type="range" class="form-control" v-model.number="gridCount" min="1" max="100">
        </div>
        <div class="form-group">
          <label for="gridColor">Grid color</label>
          <input id="gridColor" type="color" class="form-control" v-model="gridColor">
        </div>
      </div>
    </div>
    <div class="image-container" :style="`background-image: url(${imageSrc})`"></div>
    <table class="grid">
      <tbody>
        <tr v-for="i in gridCount" :key="i">
          <td
            v-for="j in gridCount"
            :key="`${i}-${j}`"
            :style="`
              width: calc(100vw / ${gridCount});
              height: calc(100vw / ${gridCount});
              border-color: ${gridColor};
            `"
          ></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  components: {
  },
  data() {
    return {
      controlsVisible: true,
      imageName: 'Choose an image',
      imageSrc: '',
      gridCount: 10,
      gridColor: 'white'
    }
  },
  methods: {
    updateImage() {
      console.log('new image selected by the user');
      console.log(this.$refs.fileInput.files);
      const fileInput = this.$refs.fileInput;
      this.imageName = fileInput.files[0].name;
      var reader = new FileReader();
      reader.onload = (e) => {
        this.imageSrc = e.target.result;
      }

      reader.readAsDataURL(fileInput.files[0]);
    }
  }
}
</script>

<style>
body {
  background: black;
  overflow: hidden;
}
.controls-container {
  position: absolute;
  z-index: 10;
}
.controls {
  background: white;
  padding: 1em;
  width: 300px;
  transition: all 0.2s ease-in-out;
}
.controls.hidden {
  opacity: 0;
}
.grid {
  position: relative;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 5;
}
td {
  border: 1px solid white;
}
.image-container {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 0;
  background-repeat: no-repeat;
  background-position: center;
  background-size: contain;

}
.fullpage {
  width: 100%;
  height: auto;
}
</style>
