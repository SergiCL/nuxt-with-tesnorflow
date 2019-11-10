<template>
  <div>
    <div v-if="imgInitialStyle" class="image-uploader-container">
      <input type="file" ref="fileInput" @change="uploadImage" style="display: none">

      <div v-show="!this.imageData.length" @click="$refs.fileInput.click()" class="drop-zone block-zone" :style="zoneInitialStyle">
        <p class="caption">{{ caption }}</p>
      </div>

      <div v-show="this.imageData.length" class="image-zone" :style="zoneInitialStyle">
        <close-icon
            @click="$refs.fileInput.click()"
            hoverText="CLick to upload another image">
        </close-icon>
        <img
            class="loaded-image"
            id="loaded-img"
            :style="imgInitialStyle"
            :src="imageData"
            alt="Uploaded image"
        >
      </div>
    </div>

    <div class="data-zone">
      <div v-if="predictions.length || isLoading" class="predictions">
        <h3>Predictions</h3>

        <loader v-show="isLoading" class="loader"></loader>

        <table v-show="!isLoading" class="prediction-table">
          <tr>
            <th class="prediction-col">Prediction</th>
            <th class="accuracy-col">Accuracy</th>
          </tr>
          <tr v-for="pred in predictions" v-bind:key="pred.className" :class="pred.class">
            <td lass="prediction-col"><div>{{ pred.className }}</div></td>
            <td class="accuracy-col"><div>{{ pred.probability.toFixed(2) }}%</div></td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
  import CloseIcon from "./CloseIcon";

  import * as mobilenet from '@tensorflow-models/mobilenet';
  import Loader from './Loader'

  export default {
    name: 'ImageUploader',
    components: {
      Loader,
      CloseIcon
    },
    props: {
      caption: {
        type: String,
        default: 'Click to upload an image'
      }
    },

    watch: {
      predictions () {
        this.isLoading = false
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

        this._predict()
      },

      async _predict () {
        this.isLoading = true
        const imgToAnalyze = document.getElementById('loaded-img')

        // Load the model.
        this.model = await mobilenet.load({
          version: 2,
          alpha: .75,
          // modelUrl: ''
          // inputRange: [0, 1]
        })

        // Classify the image.
        const numberOfResults = 5
        this.predictions = await this.model.classify(imgToAnalyze, numberOfResults)
          .then(response => {
            console.log('Response:', response)
            return response.map(predData => {
              predData['class'] = {
                'high-reliability': predData.probability*100 >= 80,
                'average-reliability': predData.probability*100 < 80 && predData.probability*100 >= 50,
                'low-reliability': predData.probability*100 < 50 && predData.probability*100 >=25,
                'very-low-reliability': predData.probability*100 < 25
              }
              return predData
            })
          })
      }
    },

    mounted () {
      this._setInitialStyles()
    },

    data () {
      return {
        imageData: '',
        zoneInitialStyle: null,
        imgInitialStyle: null,
        model: null,
        isLoading: false,
        predictions: {}
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

  .data-zone {
    .loader {
      margin-top: 82px;
    }

    .predictions {
      width: 360px;
      margin-left: auto;
      margin-right: auto;

      h3 {
        color: #8098cf;
        font-weight: normal;
        font-size: 19px;
        text-align: left;
        padding-top: 18px;
        padding-bottom: 6px;
        padding-left: 4px;
      }

      table {
        width: 100%;
        border-collapse: collapse;

        th, td {
          border-bottom: 2px solid rgba(255, 255, 255, 0.67);
          vertical-align: center;
          text-align: left;
          padding: 7px;

          > div {
            height: 25px;
            overflow: hidden;
          }
        }

        tr {
          max-height: 7px;
          &.high-reliability { background-color: rgba(132, 207, 162, 0.25); }
          &.average-reliability { background-color: rgba(173, 195, 227, 0.25); }
          &.low-reliability { background-color: rgba(207, 187, 152, 0.25); }
          &.very-low-reliability { background-color: rgba(240, 165, 161, 0.25); }
        }

        .prediction-col {
          overflow: fragments;
        }

        .accuracy-col {
          width: 25%;
          text-align: right;
          padding-right: 10px;
        }
      }
    }
  }
</style>
