<template>
  <view class="content">
    <text class="title">数字输入框：</text>
    <input-number v-model="inputValue"></input-number>
  </view>
</template>

<script>
import InputNumber from '@/components/input-number/input-number.vue'

export default {
  components: {
    InputNumber,
  },
  data () {
    return {
      inputValue: '',
    }
  },
  onLoad () { },
  methods: {},
}
</script>

<style scoped>
.content {
  padding-left: 10px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.title {
  font-size: 36rpx;
  color: #8f8f94;
}
</style>
