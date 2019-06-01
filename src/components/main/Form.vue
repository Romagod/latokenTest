<template>
  <form class="project-form" :action="action">

    <InputText id="id-1"
             placeholder="Your project name"
             :required="true"
             :validators="[
              {
                f: this.validatorMaxLength,
                data: {
                  eventType: 'input',
                  max: 5,
                  errorMessage: 'The name is too long'
                }
              },
              {
                f: this.validatorRequired,
                data: {
                  eventType: 'submit',
                  errorMessage: 'Every project needs a name, doesn’t it?'
                }
              }
             ]">
      <template v-slot:caption>
        Project name
      </template>
    </InputText>

    <InputText id="id-2"
               placeholder="Your project description"
               :required="true"
               :validators="[
              {
                f: this.validatorMaxLength,
                data: {
                  eventType: 'input',
                  max: 40,
                  errorMessage: 'Description is much length'
                }
              },
              {
                f: this.validatorRequired,
                data: {
                  eventType: 'submit',
                  errorMessage: 'Please write something here'
                }
              }
             ]">
      <template v-slot:caption>
        Short description (see examples for popular projects on <a href="#">Techcrunch</a>)
      </template>
    </InputText>

    <InputText id="id-3"
               placeholder="Your project website"
               :required="true"
               :validators="[
              {
                f: this.validatorSiteURL,
                data: {
                  eventType: 'change',
                  errorMessage: 'Invalid site name'
                }
              },
              {
                f: this.validatorRequired,
                data: {
                  eventType: 'submit',
                  errorMessage: 'Please write something here'
                }
              }
             ]">
      <template v-slot:caption>
        Website
      </template>
    </InputText>

    <Dropdown/>

    <ImageLoader
      :options="{
         maxCount: 8,
         maxSize: 5242880,
         validTypes: ['image/png', 'image/jpeg']
       }">
      <span>PNG or JPG, min 800*600, up to 5Mb</span>
    </ImageLoader>

<!--    <Submit @submit="submit"/>-->

  </form>
</template>

<script>
import InputText from './InputText'
import Submit from './Submit'
import Tooltip from './Tooltip'
import Dropdown from './Dropdown'
import ImageLoader from './ImageLoader'
export default {
  name: 'Form',
  components: {ImageLoader, Dropdown, Tooltip, Submit, InputText},
  props: [
    'action',
    'id'
  ],
  data () {
    return {

    }
  },
  methods: {
    validatorMaxLength: function (value, data) {
      return {
        result: value.length <= data.max,
        info: `${value.length}/${data.max}`,
        errorMessage: data.errorMessage
      }
    },
    validatorRequired: function (value, data) {
      return {
        result: !!value,
        errorMessage: data.errorMessage
      }
    },
    validatorSiteURL: function (value, data) {
      return {
        result: /^(?:http(s)?:\/\/)?[\w.-]+(?:\.[\w.-]+)+[\w\-._~:/?#[\]@!$&'()*+,;=.]+$/.test(value),
        errorMessage: data.errorMessage
      }
    },
    submit: function () {
      this.$emit('submit')
    }
  }

}
</script>

<style scoped>
.project-form {
  width: 580px;

  margin: 40px 0 0 0;
}
</style>
