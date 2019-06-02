<template>
  <div class="dropdown">

    <div class="dropdown__input-wrapper">
      <div class="dropdown__clear"
        v-if="inputValue.length"
        @click="clearInput">
        Banking
      </div>

      <input class="dropdown__value-field" type="text" name="dropdown"
         v-model="inputValue"
         @click="dropdownClickHandler"
         @input="filter"
         :placeholder="viewSelectedData()">
    </div>

    <div class="dropdown__drop-list-wrapper"
         v-if="dropdownIsVisible">

      <div class="dropdown__scroll"
        ref="dropdownScroll">
      </div>

      <ul class="dropdown__drop-list"
          @scroll="dropdownScrollHandler"
          :style="dropListHeight">

        <li class="dropdown__item"
            ref="item"
            v-for="(item) in dataList"
            v-if="item.visible"
            :key="item.id"
            :data-id="item.id"
            :class="{'dropdown__item--selected': item.selected}"
            @click="select(item)">
          {{ item.value }}
        </li>

      </ul>

    </div>

  </div>
</template>

<script>
export default {
  name: 'Dropdown',
  props: [
    'options'
  ],
  data () {
    return {
      'dataList': [],
      'value': [],
      'inputValue': '',
      'dropdownIsVisible': false
    }
  },
  computed: {
    dropListHeight: function () {
      const itemHeight = 45

      let countVisibleItems = this.options.maxDisplayItems < this.dataList.length
        ? this.options.maxDisplayItems : this.dataList.length

      return {'max-height': itemHeight * countVisibleItems + 'px'}
    }
  },
  methods: {
    getData: function () {
      return [
        {value: 'item-1', id: '1'},
        {value: 'foo-item-2', id: '2'},
        {value: 'bar-item-3', id: '3'},
        {value: 'baz-item-4', id: '4'},
        {value: 'foo-item-5', id: '5'},
        {value: 'item-6', id: '6'},
        {value: 'item-7', id: '7'},
        {value: 'item-8', id: '8'}
      ]
    },

    select: function (target) {
      console.log(this.value.length)

      target.selected = this.options.maxCount > this.value.length || target.selected ? !target.selected : target.selected

      if (target.selected === true && this.options.maxCount > this.value.length) {
        this.value.push(target)
      } else {
        this.value = this.value.filter(item => item !== target)
      }
    },

    filter: function () {
      this.dataList.forEach(item => {
        item.visible = item.value.startsWith(this.inputValue)
      })
    },

    dropdownClickHandler: function () {
      this.dropdownIsVisible = true

      let dropDownCloseHandler = (event) => {
        if (!~event.target.className.indexOf('dropdown')) {
          this.dropdownIsVisible = false
          document.removeEventListener('click', dropDownCloseHandler)
        }
      }

      new Promise(resolve => resolve()).then(() => {
        if (document.click !== dropDownCloseHandler) {
          document.addEventListener('click', dropDownCloseHandler)
        }
      })
    },

    dropdownScrollHandler: function (event) { // todo
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

    setScrollPosition: function (y) {
      this.$refs.dropdownScroll.style.top = y + 'px'
    },

    viewSelectedData: function () {
      let result = ''
      this.value.forEach(item => {
        console.log(item)
        result += item.value + ', '
      })

      return result.slice(0, -2) || this.options.placeholder
    },

    clearInput: function () {
      this.inputValue = ''
      this.filter()
    }
  },
  created () {
    this.dataList = this.getData()

    this.dataList.forEach(item => {
      this.$set(item, 'selected', false)
      this.$set(item, 'visible', true)
    })
  },
  mounted () {

  }
}
</script>

<style scoped>
input { /* todo */
  box-sizing: border-box;

  height: 48px;
  width: 100%;

  padding: 1em;
  margin: 0 0 0 0;

  border: 1px solid var(--inactive-color-03);
  border-radius: 2px;
  font-size: 14px;
}

.dropdown {
  position: relative;
  z-index: 1;
}

.dropdown__value-field {

}

.dropdown__input-wrapper {
  position: relative;

  display: flex;
  align-items: center;
}

.dropdown__clear {
  box-sizing: border-box;

  position: absolute;
  left: 8px;

  display: flex;
  align-items: center;

  width: 100px;
  height: 32px;

  padding: 0 0.4em;

  cursor: pointer;
  user-select: none;

  background: var(--light-color);

  border: 1px solid var(--inactive-color-04);
  border-radius: 2px;
}

.dropdown__clear:hover {
  background-color: var(--inactive-color-01);
}

.dropdown__clear:active {
  box-shadow: inset 0 0 5px 0 rgba(0,0,0,0.1);
}

.dropdown__clear::after {
  content: '';
  position: absolute;
  right: 4px;

  width: 11px;
  height: 13px;

  background: url("../../assets/basket.svg") 50% 50% no-repeat;
}

.dropdown__clear + .dropdown__value-field {
  padding-left: 120px;
}

.dropdown__drop-list-wrapper {
  position: absolute;

  width: 385px;

  border: 1px solid var(--inactive-color-04);
  overflow: hidden;
}

.dropdown__scroll {
  position: absolute;

  right: 4px;
  top: 0;

  width: 5px;
  height: 50%;

  margin: 4px 0;

  background-color: var(--inactive-color-04);

  border-radius: 3px;
}

.dropdown__drop-list {
  width: 406px;
  max-height: 200px;

  margin: 0;
  padding: 0;

  list-style: none;

  background-color: var(--light-color);

  overflow-y: scroll;
}

.dropdown__item {
  margin: 0 14px 0 0;
  padding: 1em;

  cursor: pointer;
}

.dropdown__item--selected {
  background-color: var(--inactive-color-04);
}

.dropdown__item:hover {
  background-color: var(--inactive-color-01);
}

.dropdown__item--selected:hover {
  background-color: var(--inactive-color-04);
}
</style>
