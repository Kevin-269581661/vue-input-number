<template>
  <div class="container">
    <input-number
      v-model="value1"
      :step="0.5"
      :extendParams="extendParams('a', 'b')"
      @focus="onFocus"
      @blur="onBlur"
      @change="onChange"
    ></input-number>
    <input-number
      v-model="value2"
      :min="2010"
      :max="2030"
      size="medium"
      @focus="onFocus"
      @blur="onBlur"
      @change="onChange"
    ></input-number>
    <input-number
      v-model="value3"
      :min="2000"
      :max="2050"
      :step="2"
      size="small"
      :extendParams="extendParams('e', 'f')"
      @focus="onFocus"
      @blur="onBlur"
      @change="onChangeSpecial"
    ></input-number>
    <input-number
      v-model="value4"
      :min="2000"
      :max="2050"
      :disabled="true"
      :step="0.5"
      size="mini"
      :extendParams="extendParams('g', 'h')"
      @focus="onFocus"
      @blur="onBlur"
      @change="onChange"
    ></input-number>
  </div>
</template>
<script>
import InputNumber from './inputNumber'

export default {
  components: {
    InputNumber
  },
  data () {
    return {
      value1: 2019,
      value2: 2019,
      value3: 2019,
      value4: 2019
    }
  },
  methods: {
    extendParams(paramA, paramB) {
      return { paramA, paramB }
    },
    onFocus() {
      console.log('输入框获得了焦点')
    },
    onBlur() {
      console.log('输入框失去了焦点')
    },
    onChange(value, btnType, extendParams) {
      console.log('子组件传过来的value', value)
      console.log('您点了', btnType)
      console.log('extendParams', extendParams)
    },
    onChangeSpecial(value, btnType, extendParams) {
      if (value >2018 && btnType == 'decrease') {
        this.value3 = 2018
        console.log('变个戏法')
      }
    }
  },
  watch: {
    value1(val) {
      console.log('父组件中绑定的value1', val)
    }
  },
}
</script>
<style lang="stylus">
  .container {
    margin-top 60px
  }
</style>