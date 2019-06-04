<template>
  <InputField class="dropdown"
    :validators="validators"
    :required="required"
    :name="options.name"
    :value="value">

    <template v-slot:caption>
      <slot></slot>
    </template>

    <template v-slot:default="slotProps">
      <Tooltip
        :message="slotProps.errorList[options.name]"
        :is-visible="slotProps.errorList[options.name]"
      />
      <label>
        <ul class="dropdown__selected-items"
            @click="showDroplist">

          <li class="dropdown__selected-item"
            v-for="(item) in value"
            :key="item.id">

            {{ item.value }}

            <div class="dropdown__selected-remove"
              @click="removeItem(item)">
            </div>

          </li>
          <li class="dropdown__input-item">
            <input class="dropdown__input-field" type="text"
              :placeholder="value.length ? '' : options.placeholder"
               v-model="inputValue"
              @input="filterList">
          </li>
        </ul>

        <div class="dropdown__droplist-wrapper"
           v-if="dropListIsVisible">
          <div class="dropdown__scroll"
            ref="dropdownScroll"
            v-if="calcScrollVisibility()"></div>

          <ul class="dropdown__item-list"
            :style="{'max-height': options.maxDisplayItems * 48 + 'px'}"
            @scroll.prevent="scrollHandler">

            <li class="dropdown__item"
              :ref="index"
              :id="index"
              v-for="(item, index) in dataList"
              v-if="item.visible"
              :class="{'dropdown__item--selected': item.selected}"
              :key="item.id"
              @click="selectItem(item, index)">

              {{ item.value }}

            </li>

          </ul>
        </div>
      </label>

    </template>
  </InputField>
</template>

<script>
import Tooltip from './Tooltip'
import InputField from './InputField'
export default {
  name: 'newDropdown',
  components: {InputField, Tooltip},
  props: [
    // placeholder, maxCount, maxDisplayItems, name
    'options',
    'validators',
    'required'
  ],
  data () {
    return {
      inputValue: '',
      value: [],
      dataList: {},
      dropListIsVisible: false
    }
  },
  computed: {

  },
  methods: {
    // show scroll if a count of visible items less then max display items
    calcScrollVisibility () {
      return this.dataList.filter(item => item.visible).length > this.options.maxDisplayItems
    },

    // calculate visible items and highlighting searching text
    filterList: function () {
      let position, root, currentElement, temporaryText

      for (let i = 0; i <= this.dataList.length - 1; i++) {
        // calc position searching string
        position = this.dataList[i].value.toLowerCase().indexOf(this.inputValue.toLowerCase())
        if (position !== -1) {
          this.dataList[i].visible = true

          // current li element
          currentElement = this.$refs[this.dataList[i].id - 1][0]

          // reset li content
          if (currentElement) {
            temporaryText = document.createTextNode(this.dataList[i].value)
            // eslint-disable-next-line
            while (currentElement && currentElement.firstChild) currentElement.firstChild.remove()
            currentElement.appendChild(temporaryText)
          }

          // if li element is rendered add highlight span wrapper
          if (currentElement !== undefined && this.inputValue) {
            root = currentElement.childNodes[0]

            this.dataList[i].range.setStart(root, position)
            this.dataList[i].range.setEnd(root, position + this.inputValue.length)

            let span = document.createElement('span')
            span.classList.add('highlight')

            this.dataList[i].range.surroundContents(span)
          }
        } else {
          this.dataList[i].visible = false
        }
      }
      this.$emit('input')
    },
    // show drop list if input in focus
    showDroplist: function () {
      this.dropListIsVisible = true

      let dropDownCloseHandler = (event) => {
        if (!~event.target.className.indexOf('dropdown')) {
          this.dropListIsVisible = false
          document.removeEventListener('click', dropDownCloseHandler)
        }
      }

      new Promise(resolve => resolve()).then(() => {
        if (document.click !== dropDownCloseHandler) {
          document.addEventListener('click', dropDownCloseHandler)
        }
      })
    },

    // remove item from result value
    removeItem: function (target) {
      this.value.forEach((item, index) => {
        if (item === target) {
          item.selected = false
          this.$delete(this.value, index)
          this.$emit('change', 'change')
        }
      })
    },

    // add item to result value and highlighting selected item
    selectItem: function (target) {
      target.selected = this.options.maxCount > this.value.length || target.selected ? !target.selected : target.selected

      if (target.selected === true && this.options.maxCount > this.value.length) {
        this.value.push(target)
        this.$emit('change', 'change')
      } else {
        this.value = this.value.filter(item => item !== target)
        this.$emit('change', 'change')
      }
    },

    // handle scroll event over drop list
    scrollHandler: function (event) {
      let clientHeight = event.target.clientHeight
      let scrollTop = event.target.scrollTop
      let scrollHeight = event.target.scrollHeight
      let scroll = this.$refs.dropdownScroll
      let scrollMargin = parseInt(window.getComputedStyle(scroll)['margin-top'])
      let scrollDelta = Math.floor(scrollTop) / (scrollHeight - clientHeight)

      this.setScrollPosition(
        (clientHeight - scroll.clientHeight - scrollMargin * 2) * scrollDelta
      )
    },

    // calculate drop list scroller position
    setScrollPosition: function (y) {
      this.$refs.dropdownScroll.style.top = y + 'px'
    },

    // get drop list data from server
    getData: function () {
      fetch('/getIndustries', {
        method: 'post'
      }).then(data => {
        if (data.status !== '200') {
          throw new Error('Not 200 response')
        } else {
          this.dataList = data
        }
      }).catch(() => {
        this.dataList = [
          {value: 'Agribusiness', id: '1'},
          {value: 'Agricultural Services & Products', id: '2'},
          {value: 'Auto Dealers, Japanese', id: '3'},
          {value: 'Auto Manufacturers', id: '4'},
          {value: 'Banks, Savings & Loans', id: '5'},
          {value: 'Books, Magazines & Newspapers', id: '6'},
          {value: 'Broadcasters, Radio/TV', id: '7'},
          {value: 'Builders/Residential', id: '8'},
          {value: 'Building Materials & Equipment', id: '9'},
          {value: 'Construction Services', id: '10'},
          {value: 'Credit Unions', id: '11'},
          {value: 'Crop Production & Basic Processing', id: '12'},
          {value: 'Cruise Ships & Lines', id: '13'}
        ]
      }).then(() => {
        this.dataList.forEach(item => {
          this.$set(item, 'selected', false)
          this.$set(item, 'visible', true)
          item.range = document.createRange()
        })
      })
    },
  },
  created () {
    this.getData()

    this.$parent.$on('submit', () => {
      this.$emit('submit', 'submit')
    })
  }
}
</script>

<style>
.dropdown {
  position: relative;

  width: 100%;
}

.dropdown__droplist-wrapper {
  position: absolute;

  width: 385px;

  border: 1px solid var(--inactive-color-04);
  border-radius: 4px;
  overflow: hidden;

  z-index: 1;
}

.dropdown__item-list {
  width: 406px;
  max-height: 200px;

  margin: 0;
  padding: 0;

  list-style: none;

  background-color: var(--light-color);

  overflow-y: scroll;
}

.dropdown__scroll {
  position: absolute;
  right: 4px;

  width: 4px;
  height: 50%;

  margin: 4px 0 0 0;

  background-color: var(--inactive-color-04);

  border-radius: 4px;
}

.dropdown__selected-items {
  box-sizing: border-box;

  display: flex;
  flex-wrap: wrap;
  align-items: center;
  flex: 1 1 auto;

  width: 100%;
  min-height: 48px;

  margin: 0;
  padding: 0;

  list-style: none;

  border: 1px solid var(--inactive-color-03);
  border-radius: 4px;

  font-size: 14px;
}

.dropdown__input-item {
  flex: 1 1 auto;
}

.dropdown__selected-remove {
  position: absolute;

  right: 0.8em;

  width: 11px;
  height: 13px;

  cursor: pointer;

  background: url("../../assets/basket.svg");
}

.dropdown__selected-item {
  box-sizing: border-box;
  position: relative;

  display: flex;
  align-items: center;

  height: 32px;

  margin: 8px 0.5em;
  padding: 0.2em 2em 0.2em 0.2em;

  border: 1px solid var(--inactive-color-04);
  border-radius: 4px;
}

.dropdown__input-field {
  box-sizing: border-box;
  width: 100%;
  height: 100%;

  padding: 1em;

  border: none;
}

.dropdown__input-field:focus {
  outline: none;
}

.dropdown__item {
  box-sizing: border-box;
  display: flex;

  height: 48px;

  padding: 1em;

  border-bottom: 1px solid var(--inactive-color-015);
  cursor: pointer;
}

.dropdown__item--selected {
  background-color: var(--success-color-07);
}

.dropdown__item:active {
  background-color: var(--inactive-color-04);
}

.dropdown__item:hover {
  background-color: var(--inactive-color-01);
}

.highlight {
  background-color: var(--inactive-color-01);
}
</style>
