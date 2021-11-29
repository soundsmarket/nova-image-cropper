<template>
  <div
    ref="container"
    class="container flex flex-col"
  >
    <div class="mb-4 text-left">
        <div class="my-2">Rotation: {{ rotation }}º</div>
        <input type="range" name="rotation" v-model="rotation" min="0" max="360" step="15">
    </div>
    <div class="mb-4 text-left">
        <div class="my-2">Preview</div>
        <img ref="img" :src="image" alt="Picture">
    </div>
  </div>
</template>
<script>
import Cropper from 'cropperjs'
import 'cropperjs/dist/cropper.css'

export default {
  name: 'PictureCropper',

  props: {
    image: {
      type: String,
      default: ''
    },

    ratio: {
      type: Number,
      default: 1
    }
  },

  data() {
    return {
      cropper: null,
      width: 0,
      height: 0,
      rotation: 0
    }
  },

  watch: {
    image(image) {
      if (image) {
        this.buildCropper()
      }
    },
    rotation(rotation) {
        if (this.cropper) {
            this.cropper.rotateTo(rotation)
        }
    }
  },

  mounted() {
    window.addEventListener('resize', this.setWidth)
    this.buildCropper()
  },

  destroyed() {
    window.removeEventListener('resize', this.setWidth)
    this.cropper.destroy()
  },

  methods: {
    buildCropper() {
      if (this.cropper) {
        this.cropper.destroy()
      }

      this.setWidth()
      const self = this

      this.cropper = new Cropper(this.$refs.img, {
        viewMode: 0,
        dragMode: 'crop',
        autoCropArea: 1,
        aspectRatio: self.ratio,
        checkCrossOrigin: false,
        minContainerWidth: self.width,
        minContainerHeight: self.height,
      })
      this.cropper.replace(this.image)
    },

    setWidth() {
      const width = this.$refs.container.clientWidth
      if (!width) {
        return
      }
      this.width = width
      this.height = this.ratio ? this.width / this.ratio : this.height
    },

    getCroppedCanvas() {
      if (!this.cropper) {
        return
      }
      return this.cropper.getCroppedCanvas({
        fillColor: '#fff',
      })
    },

    replace(image) {
      this.image = image

      this.buildCropper()
    }
  }
}
</script>
<style scoped>
.container {
  max-width: 640px;
  margin: 20px auto;
}
img {
  width: 100%;
  max-width: 100%;
}
</style>
