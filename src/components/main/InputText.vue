<template>
  <label class="input"
       :class="{'input--warning': hasErrors() }">

    <Tooltip
      :message="tooltip.message"
      :is-visible="tooltip.isVisible"
    />

    <div class="input__info">
      <span class="input__caption">
        <slot name="caption"></slot>
        <span class="input__is-required" v-if="required">*</span>
      </span>

      <span class="input__additional-info">
        {{ additionalInfo }}
      </span>
    </div>

    <input type="text"
           class="input__value-field"
           :id="id"
           :placeholder="placeholder"
           v-model="value"
           @input="inputHandler"
           @change="changeHandler"
           @blur="blurHandler">

  </label>
</template>

<script>
import Tooltip from './Tooltip'
export default {
  name: 'InputText',
  components: {Tooltip},
  props: [
    'id',
    'placeholder',
    'required',
    'validators',
    'submit'
  ],
  data () {
    return {
      value: '',
      additionalInfo: '',
      onInput: [],
      onChange: [],
      onSubmit: [],
      errorList: {},
      tooltip: {
        message: '',
        isVisible: false
      }
    }
  },
  watch: {
    errorList: function () {

    }
  },
  methods: {
    eventHandler: function (handlerArray) {
      handlerArray.forEach(item => {
        let validate = item.f.call(this, this.value, item.data)

        if (validate.info) {
          this.additionalInfo = validate.info
        }

        // add new error
        if (!validate.result) {
          this.getTooltip().showError(validate.errorMessage)
          this.$set(this.errorList, validate.errorMessage, item.data.eventType)
          return
        }

        // delete error
        if (this.errorList[validate.errorMessage]) {
          this.$delete(this.errorList, validate.errorMessage)
          this.getTooltip().nextError()
        }
      })
    },
    inputHandler: function () {
      this.eventHandler(this.onInput)
    },
    changeHandler: function () {
      this.eventHandler(this.onChange)
    },
    submitHandler: function () {
      this.eventHandler(this.onSubmit)
    },
    hasErrors: function () {
      return !!Object.keys(this.errorList).length
    },
    blurHandler: function () {
      // this.setTooltip(false)
    },
    getTooltip: function () {
      return {
        nextError: () => {
          for (let key in this.errorList) {
            this.tooltip.message = key
            return
          }

          this.tooltip.isVisible = false
        },
        showError: (message) => {
          this.tooltip.isVisible = true
          this.tooltip.message = message
        },
        close: () => {
          this.tooltip.isVisible = false
        }
      }
    }
  },
  created () {
    this.validators.forEach(validator => {
      let eventArray

      if (validator.data.eventType === 'input') {
        eventArray = this.onInput
      }

      if (validator.data.eventType === 'change') {
        eventArray = this.onChange
      }

      if (validator.data.eventType === 'submit') {
        eventArray = this.onSubmit
      }

      eventArray.push({
        f: validator.f,
        data: validator.data
      })

      this.inputHandler()
      this.changeHandler()
    })

    this.$parent.$on('submit', () => {
      this.submitHandler()
    })
  }
}
</script>

<style scoped>
.input {
  position: relative;
  display: block;
}

input { /* todo */
  box-sizing: border-box;

  height: 48px;
  width: 100%;

  padding: 1em;
  margin: 0 0 2em 0;

  border: 1px solid var(--inactive-color-03);
  border-radius: 2px;
  font-size: 14px;
}

input:focus {
  outline: none;
}

.input__caption {
  font-size: 12px;
}

.input__caption a {
  color: var(--link-color);
  text-decoration: none;
}

.input__info {
  display: flex;
  justify-content: space-between;

  width: 100%;

  margin: 0 0 0.3em 0;
}

.input__additional-info {
  font-size: 12px;
}

.input--warning .input__value-field {
  border: 1px solid var(--warning-color);
}

.input--warning  {
  color: var(--warning-color);
}

.input__is-required {
  color: var(--warning-color);
}
</style>
