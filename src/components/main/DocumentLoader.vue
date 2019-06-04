<template>
  <InputField class="document-loader"
    :options="options.validTypes ? options.validTypes : ['application/pdf']"
    :validators="validators"
    :required="required"
    :value="value"
    :name="options.name"
    @reset="resetHandler">

    <template v-slot:caption>
      <slot name="caption"></slot>
    </template>

    <template v-slot:default="slotProps">
      <div class="document-loader__drag-area-wrapper">

        <Tooltip
          :message="slotProps.errorList[options.name]"
          :is-visible="slotProps.errorList[options.name]"
        />

        <FileLoader
          :has-error="!!slotProps.errorList[options.name]"
          :options="options"
          :name="options.name"
          @change="fileLoaderChangeHandler">

          <template v-slot:description>
            <slot name="description"></slot>
          </template>

        </FileLoader>

      </div>
    </template>
  </InputField>
</template>

<script>
import InputField from './InputField'
import FileLoader from './FileLoader'
import Tooltip from './Tooltip'
export default {
  name: 'DocumentLoader',
  components: {Tooltip, FileLoader, InputField},
  props: [
    // maxSize, validTypes, name
    'options',
    'validators',
    'required'
  ],
  data () {
    return {
      value: {}
    }
  },
  methods: {
    fileLoaderChangeHandler: function (document) {
      this.value = {document}
      this.$emit('change', 'change')
    },
    resetHandler: function () {
      this.value = {}
      this.$parent.$emit('reset')
    }
  },
  created () {
    this.$parent.$on('submit', () => {
      this.$emit('submit', 'submit')
    })
  }
}
</script>

<style scoped>
.document-loader__drag-area-wrapper {
  position: relative;

  display: flex;
}
</style>
