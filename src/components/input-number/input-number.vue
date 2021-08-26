<!--
 * @Author: Simon
 * @Date: 2021-08-24 12:26:51
 * @LastEditors: Simon
 * @LastEditTime: 2021-08-26 16:28:25
 * @Description: input-number 数字输入框
-->
<template>
  <view class="input-number">
    <input
      :type="type"
      :value="selfValue"
      :placeholder="placeholder"
      :placeholder-style="placeholderStyle"
      :disabled="disabled"
      @input="_inputChange"
    />
  </view>
</template>

<script>
export default {
  name: 'input-number',
  props: {
    type: {
      type: String,
      default: 'number'
    },
    value: {
      default: ''
    },
    max: { // 最大值
      type: Number
    },
    min: { // 最小值
      type: Number
    },
    precision: { // 数值精度
      type: Number
    },
    placeholder: {
      type: String,
      default: ''
    },
    placeholderStyle: {
      type: String
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      selfValue: ''
    }
  },
  watch: {
    value (newValue) {
      this.selfValue = newValue
    }
  },
  created () {
    this.selfValue = this.value
  },
  methods: {
    _inputChange (e) {
      let inputValue = e.detail.value.toString()
        .replace(/[^\d\.-]/g, "") // 去掉除数字、逗号、负号外的其它字符
        .replace(".", "$#$")
        .replace(/\./g, "")
        .replace("$#$", ".") // 去掉多余的小数点
      const pointIndex = inputValue.indexOf('.')

      if (pointIndex === 0) { // 首位小数点情况
        inputValue = inputValue.replace(/^(\.)(\d*)(\1*)/g, "0$1$2")
      }
      if (~pointIndex && this.precision && this.precision >= 0) { // precision位小数
        inputValue = inputValue.substring(0, pointIndex + this.precision + 1)
      } else if (~pointIndex && this.precision && this.precision === 0) { // 整数
        inputValue = inputValue.replace(/\./g, "")
      }

      if (this.max && Number(inputValue) > this.max) {
        inputValue = this.max.toString()
      }
      if (this.min && Number(inputValue) < this.min) {
        inputValue = this.min.toString()
      }

      // this.$nextTick(() => {
      this.selfValue = inputValue
      this.$emit('input', this.selfValue)
      return this.selfValue // 小程序端return字符串替换input内容
      // })
    }
  }
}
</script>
