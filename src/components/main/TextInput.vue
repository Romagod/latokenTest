<template>
  <InputField
    class="text"
    :validators="validators"
    :required="required"
    :inputGroup="true"
    :value="value">

    <template v-slot:caption>
      <slot></slot>
    </template>
    <template v-slot:default="slotProps">
      <label>
        <div class="text__input-wrapper"
          v-for="item in renderedItems"
          :key="item.name">

          <input class="text__input-field" type="text"
             v-if="item.type === 'text'"
             v-model="value[item.name]"
            :has-error="!!slotProps.errorText"
            :name="item.name"
            :placeholder="item.placeholder"
            @input="inputHandler"
            @change="changehandler">

          <textarea class="text__textarea-field"
            v-if="item.type === 'textarea'"
            v-model="value[item.name]"
            :has-error="!!slotProps.errorText"
            :name="item.name"
            :placeholder="item.placeholder"
            @input="inputHandler"
            @change="changehandler">
          </textarea>

          <Tooltip
            :message="slotProps.errorText"
            :is-visible="slotProps.errorText"
          />
        </div>

      </label>
    </template>
  </InputField>

</template>

<script>
import InputField from './InputField'
import Tooltip from './Tooltip'
export default {
  name: 'Test',
  components: {
    InputField,
    Tooltip
  },
  props: [
    'renderedItems',
    'validators',
    'required',
    'tooltip'
  ],
  data () {
    return {
      value: {}
    }
  },
  methods: {
    inputHandler: function () {
      this.$emit('input', 'input')
    },
    changehandler: function () {
      this.$emit('change', 'change')
    }
  },
  created () {
    this.$parent.$on('submit', () => {
      this.$emit('submit', 'submit')
    })

    let item
    for (item in this.renderedItems) {
      this.$set(this.value, this.renderedItems[item].name, '')
    }
  }
}
</script>

<style scoped>
.text {
  margin: 2em 0 0 0;
}

.text__input-wrapper {
  position: relative;

  display: flex;
}

.text__input-wrapper:not(:first-child) {
  margin: 0.5em 0 0 0;
}

.text__input-field {
  box-sizing: border-box;

  height: 48px;
  width: 100%;

  padding: 1em;

  border: 1px solid var(--inactive-color-03);
  border-radius: 2px;
  font-size: 14px;
}

.text__input-field:focus {
  outline: none;
}

.text__textarea-field {
  box-sizing: border-box;
  width: 100%;
  height: 272px;

  padding: 1em;

  border-radius: 4px;
  border: 1px solid var(--inactive-color-03);

  resize: none;

  font-size: inherit;
  font-family: inherit;
}

.text__textarea-field:focus {
  outline: none;
}
</style>
