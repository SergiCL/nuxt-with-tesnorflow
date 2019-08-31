<template>
  <section class="container">
    <div>
      <h1 class="title">Image recognition</h1>
      <h2 class="subtitle">A test project using tensorflow.js and Nuxt</h2>
      <image-uploader></image-uploader>
    </div>
  </section>
</template>

<script>
import TensorflowExample from '~/components/TensorflowExample'
import ImageUploader from "../components/ImageUploader"

export default {
  components: {
    TensorflowExample,
    ImageUploader
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  text-align: center;
}

.title {
  font-weight: 200;
  font-size: 87px;
  color: #4c6883;
  text-align: justify;
}

.subtitle {
  font-weight: 300;
  font-size: 29px;
  color: #8098cf;
  text-align: right;
  padding-bottom: 30px;
  padding-right: 5px;
}
</style>

