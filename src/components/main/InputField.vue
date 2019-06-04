<template>
  <div class="input"
         :class="{'input--warning': hasErrors() }">

    <div class="input__info">
      <span class="input__caption">
        <slot name="caption"></slot>
        <span class="input__is-required" v-if="required">*</span>
      </span>

      <span class="input__additional-info">
        {{ additionalInfo }}
      </span>
    </div>

    <slot :errorText="getErrorText()">
    </slot>

</div>
</template>

<script>
import Tooltip from './Tooltip'
export default {
  name: 'InputField',
  components: {Tooltip},
  props: [
    'value',
    'required',
    'validators',
    'submit',
    'inputGroup',
    'name'
  ],
  data () {
    return {
      additionalInfo: '',
      errorList: {},
      tooltip: {
        message: '',
        isVisible: false
      }
    }
  },
  methods: {
    // handler for all input events
    eventHandler: function (eventType) {
      let index

      // searching all validators for input eventType
      this.validators.map(validator => {
        if (validator.data.eventType !== eventType) return

        // division logic handling for component having one or some input fields
        if (this.inputGroup) {
          for (index in this.value) {
            this.validateItem(validator, this.value[index], index)
          }
        } else {
          this.validateItem(validator, this.value, this.name)
        }
      })
    },

    // validate item
    validateItem: function (validator, item, name) {
      let validateResult = validator(item, validator.data)

      if (validateResult.info) {
        this.additionalInfo = validateResult.info
      }

      let errorName = name + validator.data.eventType

      // add new error
      if (!validateResult.result) {
        if (!this.errorList[name]) {
          this.$set(this.errorList, name, {})
        }

        this.$set(this.errorList[name], errorName, validateResult.errorMessage)

        this.$parent.$emit('error', {name: name, hasErrors: this.hasErrors()})

      // delete error
      } else if (this.errorList[name]) {
        this.$delete(this.errorList[name], errorName)
        this.$delete(this.errorList[name], name + 'submit')
        this.$parent.$emit('error', {name: name, hasErrors: this.hasErrors()})
      }
    },

    // check error in errorList
    hasErrors: function () {
      for (let type in this.errorList) {
        for (let key in this.errorList[type]) {
          return true
        }
      }
      return false
    },
    getErrorText: function () {
      let result = false
      for (let type in this.errorList) {
        for (let key in this.errorList[type]) {
          result = this.errorList[type][key]
        }
      }

      return result
    }
  },
  created () {
    this.validators.map(item => {
      if (item.data.eventType !== 'submit') {
        this.eventHandler(item.data.eventType)
      }

      this.$parent.$on(item.data.eventType, (type) => {
        this.$nextTick(this.eventHandler(type))
      })
    })

    this.$parent.$on('delete', (id) => {
      this.$emit('delete', id)
    })

    this.$parent.$on('reset', () => {
      this.$emit('reset')
    })
  }
}
</script>

<style scoped>
.input {
  position: relative;
  display: block;

  margin: 2em 0 0 0;
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

input[has-error],
textarea[has-error] {
  border: 1px solid var(--warning-color);
}

div[has-error] {
  border: 1px dashed var(--warning-color);
}

.input--warning .input__info {
  color: var(--warning-color);
}

.input__is-required {
  color: var(--warning-color);
}
</style>
