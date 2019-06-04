<template>
  <div class="file-loader"
    v-if="!isFull"
    :class="{[uploadStatus]: true}"
    @drag="prevent"
    @dragleave="prevent"
    @dragover="prevent"
    @drop="dropHandler">

    <Tooltip
      class="file-loader__tooltip"
      :message="tooltip.message"
      :is-visible="tooltip.isVisible"/>

    <div class="file-loader__reset"
      v-if="Object.keys(value).length"
      @click="reset"></div>

    <input type="file" ref="input" class="hidden"
      :name="name"
      :multiple="multiple ? multiple : null">

    <div class="file-loader__download-ico"></div>
    <div class="file-loader__download-description">

      <slot name="description"
        v-if="uploadStatus !== 'file-loader--success'">

      </slot>

      <div class="file-loader__status"
        v-if="uploadStatus === 'file-loader--success' && !multiple">
          Uploaded
      </div>

    </div>

  </div>
</template>

<script>
import Tooltip from './Tooltip'
export default {
  name: 'FileLoader',
  components: {Tooltip},
  props: [
    'name',
    'description',
    'options',
    'multiple'
  ],
  data () {
    return {
      value: {},
      tooltip: {
        message: '',
        isVisible: false
      },
      uploadStatus: false
    }
  },
  methods: {
    // prevent to default some of drag events
    prevent: function (event) {
      event.stopPropagation()
      event.preventDefault()
    },

    // drop item handler
    dropHandler: function (event) {
      event.stopPropagation()
      event.preventDefault()

      let dt = event.dataTransfer
      let files = dt.files

      // add new result value
      if (this.multiple == null) {
        // validate new item
        if (
          !!~this.options.validTypes.indexOf(files[0].type) && this.options.maxSize >= files[0].size) {
          this.$set(this.value, 0, files[0])

          this.$emit('change', files[0])

          this.uploadStatus = 'file-loader--success'

          this.tooltip.isVisible = false
        } else {
          this.tooltip.message = 'File is not valid'
          this.tooltip.isVisible = true

          this.uploadStatus = 'file-loader--fail'
        }
        // exit from function
        return
      }

      // add new array result value
      [].forEach.call(files, file => {
        // validate new items
        // eslint-disable-next-line
        if (
          !!~this.options.validTypes.indexOf(file.type) &&
          !this.isFull &&
          this.options.maxSize >= file.size
        ) {
          file.id = Math.random().toString(16).slice(2)

          this.$set(this.value, file.id, file)

          this.uploadStatus = ''

          this.tooltip.isVisible = false

          this.$emit('change', file)
        } else {
          this.uploadStatus = 'file-loader--fail'
          this.tooltip.message = 'File is not valid'
          this.tooltip.isVisible = true
        }
      })
    },

    // submit event handler
    onSubmit: function () {
      let dt = new DataTransfer()

      for (let key in this.value) {
        dt.items.add(this.value[key])
      }

      this.$refs['input'].files = dt.files
    },

    // delete event handler
    onDelete: function (id) {
      this.$delete(this.value, id)
    },

    // reset result value
    reset: function () {
      this.value = {}
      this.$parent.$emit('reset')
      this.uploadStatus = ''
    }
  },
  computed: {
    // check to result value has max count items
    isFull: function () {
      return Object.keys(this.value).length >= this.options.maxCount
    }
  },
  created () {
    this.$parent.$on('delete', this.onDelete)
    this.$parent.$on('submit', this.onSubmit)
  }
}
</script>

<style scoped>
.file-loader {
  position: relative;

  box-sizing: border-box;

  display: flex;
  align-items: center;

  height: 72px;
  width: 100%;

  padding: 2em;

  background-color: var(--light-color);

  border: 1px dashed var(--inactive-color-04);
  border-radius: 6px;
}

.file-loader__tooltip {
  margin: 5px 0 0 0;
  align-self: flex-end;
}

.file-loader--success {
  background-color: var(--success-color-07);
  border:1px solid var(--success-color);
}

.file-loader--fail {
  border:1px dashed var(--warning-color);

  color: var(--light-color)
}

.file-loader--fail .file-loader__status {
  color: var(--light-color)
}

.file-loader__reset {
  position: absolute;
  right: 2em;

  width: 20px;
  height: 20px;

  cursor: pointer;

  background: url("../../assets/basket.svg") no-repeat 50% 50%;
}

.file-loader__download-ico {
  width: 30px;
  height: 20px;

  margin: 0 10px 0 0;

  background: url("../../assets/download.svg") 50% 50% no-repeat;
}

.file-loader--success .file-loader__download-ico {
  background: url("../../assets/success.svg") 50% 50% no-repeat;
  background-size: 100% 100%;
}

.file-loader__download-description {
  user-select: none;

  color: var(--inactive-color-04)
}
.hidden {
  display: none;
}
</style>
