<template>
  <InputField class="image-loader"
      :options="options.validTypes ? options.validTypes : ['image/png', 'image/jpeg']"
      :validators="validators"
      :required="required"
      :name="options.name"
      :value="value"
      @reset="resetHandler">

    <template v-slot:caption>
      <slot name="caption"></slot>
    </template>

    <template v-slot:default="slotProps">
      <div class="image-loader__drag-area-wrapper">
      <FileLoader
        :has-error="!!slotProps.errorList[options.name]"
        :options="options"
        :multiple="'multiple'"
        :name="options.name"
        @change="fileLoaderChangeHandler">

        <template v-slot:description>
          <slot name="description"></slot>
        </template>

      </FileLoader>

      <Tooltip
        :message="slotProps.errorList[options.name]"
        :is-visible="slotProps.errorList[options.name]"
      />

      </div>

      <ul class="image-loader__file-list">

        <li class="image-loader__item"
          v-if="images[image.id]"
          v-for="(image, index) in value"
          :key="index">

          <img class="image-loader__image"
            :src="images[image.id]">

          <div class="image-loader__image-name">
            {{ image.name }}
          </div>

          <div class="image-loader__basket"
            @click="removeImage(image.id)">

          </div>

        </li>

      </ul>
    </template>

  </InputField>
</template>

<script>
import InputField from './InputField'
import FileLoader from './FileLoader'
import Tooltip from './Tooltip'

export default {
  name: 'ImageLoader',
  components: {Tooltip, FileLoader, InputField},
  props: [
    // maxCount, maxSize, validTypes, name
    'options',
    'validators',
    'required'
  ],
  data () {
    return {
      value: {},
      images: {}
    }
  },
  methods: {
    // read image and reject reading result
    readImage: function (image) {
      return new Promise((resolve) => {
        let reader = new FileReader()
        reader.onload = () => {
          resolve(reader.result)
        }
        reader.readAsDataURL(image)
      })
    },

    // remove image from result and page
    removeImage: function (id) {
      this.$delete(this.value, id)
      this.$delete(this.images, id)

      this.$emit('delete', id)
      this.$emit('change', 'change')
    },

    // add new value to result and read image
    fileLoaderChangeHandler: function (image) {
      this.$set(this.value, image.id, image)
      this.$emit('change', 'change')

      this.readImage(image).then((result) => {
        this.$set(this.images, image.id, result)
      })
    },

    // remove all images from result and page
    resetHandler: function () {
      for (let imageId in this.value) {
        this.$delete(this.value, imageId)
      }
    }
  },
  created () {
    console.log(this.$parent)

    this.$parent.$on('submit', () => {
      this.$emit('submit', 'submit')
    })
  }
}

</script>

<style scoped>
.image-loader {
  position: relative;
  display: block;
}

.image-loader__file-list {
  margin: 8px 0 0 0;
  padding: 0;

  list-style: none;
}

.image-loader__drag-area-wrapper {
  position: relative;

  display: flex;
}

.image-loader__item {
  position: relative;
  display: flex;
  align-items: center;

  padding: 5px 0;

  border-bottom: 1px solid var(--inactive-color-01);

  font-size: 12px;
}

.image-loader__basket {
  position: absolute;
  right: 16px;

  background: url("../../assets/basket.svg") 50% 50% no-repeat;

  width: 15px;
  height: 15px;

  cursor: pointer;
}

.image-loader__image {
  display: block;

  width: 54px;
  height: 36px;

  margin: 0 16px 0 0;

  object-fit: cover;

  border-radius: 4px;
}
</style>
