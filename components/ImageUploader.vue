<template>
  <div v-if="imgInitialStyle" class="image-uploader-container">
    <div v-show="!this.imageData.length" @click="$refs.fileInput.click()" class="drop-zone block-zone" :style="zoneInitialStyle">
      <input type="file" ref="fileInput" @change="uploadImage" style="display: none">
      <p class="caption">{{ caption }}</p>
    </div>

    <div v-show="this.imageData.length" class="image-zone" :style="zoneInitialStyle">
      <close-icon @onClose="deleteImage"></close-icon>
      <img
          class="loaded-image"
          id="loaded-img"
          :style="imgInitialStyle"
          :src="imageData"
          alt="Uploaded image"
      >
    </div>
  </div>
</template>

<script>
  import CloseIcon from "./CloseIcon";
  export default {
    name: 'ImageUploader',
    components: {
      CloseIcon
    },
    props: {
      caption: {
        type: String,
        default: 'Click to upload an image'
      }
    },

    methods: {
      _setInitialStyles () {
        const width = '360px';
        const height = '240px';

        this.zoneInitialStyle = {
          width: width,
          height: height
        }

        this.imgInitialStyle = {
          'max-width': width,
          'max-height': height
        }
      },

      uploadImage (event) {
        // Reference to the DOM input element
        var input = event.target
        // Ensure that you have a file before attempting to read it
        if (input.files && input.files[0]) {
          // create a new FileReader to read this image and convert to base64 format
          var reader = new FileReader()
          // Define a callback function to run, when FileReader finishes its job
          reader.onload = (e) => {
            // Note: arrow function used here, so that "this.imageData" refers to the imageData of Vue component
            // Read image as base64 and set to imageData
            this.imageData = e.target.result
          }
          // Start the reader job - read file as a data url (base64 format)
          reader.readAsDataURL(input.files[0])
        }
      },

      deleteImage () {
        this.imageData = ''
      }
    },

    mounted () {
      this._setInitialStyles()
      this.isMounted = true
    },

    data () {
      return {
        imageData: '',
        zoneInitialStyle: null,
        imgInitialStyle: null
      }
    }
  }
</script>

<style scoped lang="scss">
.image-uploader-container {
  display: flex;
  align-items: center;
  justify-content: center;

  .drop-zone {
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #F6F6F6;
    border: 1px dotted #8098cf;
    border-radius: 1px;

    .caption {
      color: dimgray;
    }

    &:hover {
      background-color: #8098cf;
      cursor: pointer;

      .caption {
        color: #F6F6F6;
      }
    }


  }
}
</style>
