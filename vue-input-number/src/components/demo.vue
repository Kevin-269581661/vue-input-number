<template>
  <div class="container">

    <input-number
      v-model="value1"
      @focus="onFocus"
      @blur="onBlur"
      @change="onChange"
      :extendParams="1"
    ></input-number>

    <input-number
      v-model="value2"
      :min="2010"
      :max="2030"
      :step="1.5"
      size="medium"
      @focus="onFocus"
      @blur="onBlur"
      @change="onChange"
      :extendParams="2"
    ></input-number>

    <ul>
      <li v-for="(item, index) in list" :key="index">
        <input-number
          v-model="item.value"
          :min="2000"
          :max="2050"
          :step="2"
          size="small"
          :extendParams="extendParams(item.name, index)"
          @focus="onFocus"
          @blur="onBlur"
          @change="onChangeSpecial"
        ></input-number>
      </li>
    </ul>
 
    <input-number
      v-model="value4"
      :min="2000"
      :max="2050"
      :disabled="true"
      :step="0.5"
      size="mini"
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
      value4: 2019,
      list: [
        {
          value: 2019,
          name: 'aa'
        },
        {
          value: 2020,
          name: 'bb'
        }
      ]
    }
  },
  methods: {
    extendParams (name, index) {
      return { name, index }
    },
    onFocus () {
      console.log('输入框获得了焦点')
    },
    onBlur () {
      console.log('输入框失去了焦点')
    },
    onChange (value, btnType, extendParams) {
      console.log('改变后的值是', value)
      console.log('您刚刚点了按钮是', btnType)
      console.log('传入的参数是', extendParams)
      if (value > 2018 && btnType == 'decrease' && extendParams == 2) {
        this.value2 = 2018
        console.log('您点了减按钮，我猜你是想回到2018')
      }
    },
    onChangeSpecial (value, btnType, extendParams) {
      console.log('改变后的值是', value)
      console.log('您刚刚点了按钮是', btnType)
      console.log('传入的参数是', extendParams)
      // 这里可以进行一些更多的对数据的判断和操作
    }
  }
}
</script>
<style lang="stylus">
  .container {
    margin-top 60px
  }
</style>