<template>
  <label class="image-loader"
    :options="options.validTypes ? options.validTypes : ['image/png', 'image/jpeg']">

    <div class="image-loader__drag-area"
      v-if="!isFull"
      ref="dragArea"
      @drag="prevent"
      @dragleave="prevent"
      @dragover="prevent"
      @drop="dropHandler"
    >
      <div class="image-loader__download-ico"></div>
      <div class="image-loader__download-description">
        <slot></slot>
      </div>
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

  </label>
</template>

<script>
export default {
  name: 'ImageLoader',
  props: [
    'options'
  ],
  data () {
    return {
      value: {},
      images: {}
    }
  },
  computed: {
    isFull: function () {
      return Object.keys(this.value).length >= this.options.maxCount
    }
  },
  methods: {
    prevent: function (event) {
      event.stopPropagation()
      event.preventDefault()
    },
    dropHandler: function (event) {
      event.stopPropagation()
      event.preventDefault()

      let dt = event.dataTransfer
      let files = dt.files

      ;[].forEach.call(files, image => {
        // eslint-disable-next-line
        if (
          !!~this.options.validTypes.indexOf(image.type) &&
          !this.isFull &&
          this.options.maxSize >= image.size
        ) {
          image.id = Math.random().toString(16).slice(2)

          this.readImage(image).then((result) => {
            this.$set(this.images, image.id, result)
            this.$set(this.value, image.id, image)
          })
        } else {
          console.log('error') // todo
        }
      })
    },
    readImage: function (image) {
      return new Promise((resolve) => {
        let reader = new FileReader()
        reader.onload = () => {
          resolve(reader.result)
        }
        reader.readAsDataURL(image)
      })
    },
    removeImage: function (id) {
      this.$delete(this.value, id)
      this.$delete(this.images, id)
    }
  }
}
</script>

<style scoped>
.image-loader__drag-area {
  box-sizing: border-box;

  display: flex;
  align-items: center;

  height: 72px;

  margin-top: 2em; /* todo */
  padding: 2em;

  background-color: var(--light-color);

  border: 1px dashed var(--inactive-color-04);
  border-radius: 6px;
}

.image-loader__download-ico {
  width: 30px;
  height: 20px;

  margin: 0 10px 0 0;

  background: url("../../assets/download.svg") 50% 50% no-repeat;
}

.image-loader__download-description {
  color: var(--inactive-color-04)
}

.image-loader__file-list {
  margin: 8px 0 0 0;
  padding: 0;

  list-style: none;
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
