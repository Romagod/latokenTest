<template>
  <div class="dropdown">

    <input class="dropdown__value-field" type="text" name="asd"
      v-model="inputValue"
      @click="dropdownClickHandler"
      @input="filter">

    <div class="dropdown__drop-list-wrapper"
         v-if="dropdownIsVisible">

      <div class="dropdown__scroll"
        ref="dropdownScroll">
      </div>

      <ul class="dropdown__drop-list"
          @scroll="dropdownScrollHandler">

        <li class="dropdown__item"
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
  data () {
    return {
      'dataList': [],
      'value': [],
      'inputValue': '',
      'dropdownIsVisible': false,
      'listLength': 5
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
      target.selected = !target.selected

      if (target.selected === true) {
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

      setTimeout(() => {
        if (document.click !== dropDownCloseHandler) {
          document.addEventListener('click', dropDownCloseHandler)
        }
      }, 0)
    },
    dropdownScrollHandler: function (event) {
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
    }
  },
  created () {
    this.dataList = this.getData()

    this.dataList.forEach(item => {
      this.$set(item, 'selected', false)
      this.$set(item, 'visible', true)
    })
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
