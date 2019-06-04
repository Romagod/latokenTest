<template>
  <form class="project-form"
        ref="form"
        :action="action">
    <TextInput
      :validators="[
        newValidator(validatorMaxLength, {
          eventType: 'input',
          max: 50,
          errorMessage: 'The name is too long'
        }),
        newValidator(validatorRequired, {
          eventType: 'submit',
          errorMessage: 'Every project needs a name, doesn’t it?'
        })
      ]"
      :required="true"
      :renderedItems="[{name: 'project-name', type: 'text', placeholder: 'Your project name'}]"
      @error="errorHandler">
      Project name
    </TextInput>

    <TextInput
      :validators="[
        newValidator(validatorRequired, {
          eventType: 'submit',
          errorMessage: 'Please write something here'
        }),
        newValidator(validatorMaxLength, {
          eventType: 'input',
          max: 40,
          errorMessage: 'The description is too long'
        })
        ]"
        :required="true"
        :renderedItems="[{name: 'short-description', type: 'text', placeholder: 'Short description'}]"
        @error="errorHandler">
      Short description (see examples for popular projects on <a href="#">Techcrunch</a>)
    </TextInput>

    <TextInput
      :validators="[
        newValidator(validatorRegExp, {
          eventType: 'change',
          regExp: /^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w.-]+)+[\w\-._~:/?#[\]@!$&'()*+,;=.]+$/,
          errorMessage: 'It now valid site URL'
        }),
        newValidator(validatorRequired, {
          eventType: 'submit',
          errorMessage: 'It can not be that you don’t have a website'
        })
        ]"
      :required="true"
      :renderedItems="[{name: 'short-description', type: 'text', placeholder: 'Your project website'}]"
      @error="errorHandler">
      Website
    </TextInput>

    <TextInput
      :validators="[
        newValidator(validatorMaxLength, {
          eventType: 'input',
          max: 1000,
          errorMessage: 'The description is too long'
        }),
        newValidator(validatorRequired, {
          eventType: 'submit',
          errorMessage: 'Describe what are you doing'
        })
        ]"
      :required="true"
      :renderedItems="[{name: 'full-description', type: 'textarea', placeholder: 'Full description'}]"
      @error="errorHandler">
      Website
    </TextInput>

    <Dropdown
      :options="{
        placeholder: 'Choose up to 3 industries that describes your project the best',
        maxCount: 5,
        maxDisplayItems: 6,
        name: 'dropdown'
      }"
      :validators="[
        newValidator(validatorMaxCount, {
          eventType: 'change',
          max: 5,
          min: 1,
          errorMessage: 'Field has too much items'
        }),
        newValidator(validatorRequired, {
          eventType: 'submit',
          errorMessage: 'Select at least one industry'
        })
      ]"
      @error="errorHandler">

      Industries
    </Dropdown>

    <div class="project-form__input-group">
      <DocumentLoader
        :options="{
         maxSize: 52428800,
         validTypes: ['application/pdf'],
         name: 'documentLoader',
       }"
        :validators="[]"
        @error="errorHandler">
        <template v-slot:description>PDF, up to 50Mb</template>
        <template v-slot:caption>Whitepaper</template>
      </DocumentLoader>

      <DocumentLoader
        :required="true"
        :options="{
          maxSize: 52428800,
          validTypes: ['application/pdf'],
          name: 'documentLoader',
        }"
        :validators="[
        newValidator(validatorRequired, {
          eventType: 'submit',
          errorMessage: 'No pitch deck? Do you even know where you are?'
          })
        ]"
        @error="errorHandler">
        <template v-slot:description>PDF, up to 50Mb</template>
        <template v-slot:caption>Pitch deck (see <a href="#">template)</a></template>
      </DocumentLoader>
    </div>

    <ImageLoader
      :options="{
         maxCount: 8,
         maxSize: 5242880,
         validTypes: ['image/png', 'image/jpeg'],
         name: 'ImageLoader'
       }"
      :required="true"
      :validators="[
        newValidator(validatorMaxCount, {
          eventType: 'change',
          max: 8,
          errorMessage: 'Field has too much items'
        }),
        newValidator(validatorRequired, {
          eventType: 'submit',
          errorMessage: 'Field not must be empty'
        })
      ]"
      @error="errorHandler">
      <template v-slot:description>PNG or JPG, min 800*600, up to 5Mb</template>
      <template v-slot:caption>Images about project</template>
    </ImageLoader>

    <TextInput
      :validators="[
        newValidator(validatorRegExp, {
          eventType: 'input',
          regExp: /^(https?:\/\/)?((www\.)?(youtube|vimeo)\.com|youtu\.?be)\/.+$/,
          errorMessage: 'Whatever it is, this is definitely not a link to the video'
        }),
        newValidator(validatorCount, {
          eventType: 'change',
          count: 2,
          errorMessage: 'It is not valid'
        })]"
      :renderedItems="[
          {name: 'video-1', type: 'text', placeholder: 'Youtube or Vimeo link'},
          {name: 'video-2', type: 'text', placeholder: 'Youtube or Vimeo link'}
        ]"
      @error="errorHandler">
      Videos about project
    </TextInput>

    <div class="project-form__input-group">
      <Button
        :value="'Back'"
        @click="back"/>

      <Button
        :value="'Continue'"
        :buttonStyle="'main'"
        @click="submit"/>
    </div>
  </form>
</template>

<script>
import TextInput from './TextInput'
import Tooltip from './Tooltip'
import Dropdown from './Dropdown'
import ImageLoader from './ImageLoader'
import InputField from './InputField'
import Button from './Button'
import DocumentLoader from './DocumentLoader'

export default {
  name: 'Form',
  components: {DocumentLoader, Button, TextInput, InputField, ImageLoader, Dropdown, Tooltip},
  props: [
    'action',
    'id'
  ],
  data () {
    return {
      errorList: {}
    }
  },
  methods: {
    // validator "constructor"
    newValidator: function (F, data) {
      return new F(data)
    },

    // checking max items length. It's check count of items in object and arrays or string length for string
    validatorMaxLength: function (data) {
      function validator (value) {
        return {
          result: (value.length || Object.keys(value).length) <= data.max,
          info: `${value.length}/${data.max}`,
          errorMessage: data.errorMessage
        }
      }
      validator.data = data
      return validator
    },

    // checking item for fill fulled
    validatorRequired: function (data) {
      function validator (value) {
        return {
          result: Object.keys(value).length,
          errorMessage: data.errorMessage
        }
      }
      validator.data = data
      return validator
    },

    // check item on success for regexp checking
    validatorRegExp: function (data) {
      function validator (value) {
        return {
          result: data.regExp.test(value) || value === '',
          errorMessage: data.errorMessage
        }
      }
      validator.data = data
      return validator
    },

    // check max count of items
    validatorMaxCount: function (data) {
      function validator (value) {
        return {
          result: (value.length || Object.keys(value).length) <= data.max,
          info: `Max ${data.max}${data.min ? `, at least ${data.min} required` : ''}`,
          errorMessage: data.errorMessage
        }
      }
      validator.data = data
      return validator
    },

    // check count of items
    validatorCount: function (data) {
      function validator () {
        return {
          result: true,
          info: `Up to ${data.count} videos, not required`,
          errorMessage: data.errorMessage
        }
      }
      validator.data = data
      return validator
    },

    // submit form and redirect to nex page if form has not errors
    submit: function () {
      this.$emit('submit', 'submit')
      let formData = new FormData(this.$refs['form'])

      if (!this.hasErrors()) {
        fetch('/', {
          method: 'post',
          body: formData
        }).then(data => {
          if (data.status !== '200') {
            throw new Error('Not 200 response')
          } else {
            // go to next page
          }
        }).catch((err) => {
          console.error(err)
          // error handler
        })
      }
    },

    // get error from child inputs components
    errorHandler: function (data) {
      this.errorList[data.name] = data.hasErrors
    },

    // checking form errors
    hasErrors: function () {
      for (let key in this.errorList) {
        if (this.errorList[key] === true) {
          return true
        }
      }

      return false
    },

    // click handler for button 'back'
    back: function () {
      window.history.back() // todo router
    }
  }
}
</script>

<style scoped>
.project-form {
  width: 580px;

  margin: 40px 0 0 0;
}

.project-form__input-group {
  display: flex;
}

.project-form__input-group .document-loader {
  width: 100%;
}

.project-form__input-group .button-component:first-child {
  flex: 0.35 1 auto;
}

.project-form__input-group .button-component:last-child {
  flex: 0.65 1 auto;
}

.project-form__input-group div:first-child {
  margin-right: 1em;
}

</style>
