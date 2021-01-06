<template>
  <!-- 内联样式  -->
  <div :style="{'display': 'flex', 'flex-direction': 'column'}">
    <div class="class-title">{{ title }}</div>
    <p>Original message: "{{ message }}"</p>
    <p>Computed reversed message: "{{ reversedMessage }}"</p>
    <p>Reversed message: "{{ reversedMessageMethod() }}"</p>
    <div>
      Ask a yes/no question:
      <input v-model="question">
      <p>{{ answer }}</p>
    </div>
  </div>
</template>

<script>
import debounce from 'lodash.debounce'

const items = [{ message: 'Foo' }, { message: 'Bar' }]
export default {
  data () {
    // data functions should return an object
    return {
      items,
      title: '练习2',
      message: 'Hello',
      question: '',
      answer: 'I cannot give you an answer until you ask a question!'
    }
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('items is: ' + JSON.stringify(this.items))
     // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    this.debouncedGetAnswer = debounce(this.getAnswer, 500)
  },
  // 计算属性
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  },
  // 侦听属性
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      console.log(newQuestion, oldQuestion)
      this.answer = 'Waiting for you to stop typing...'
      // this.getAnswer()
      this.debouncedGetAnswer()
    }
  },
  // 方法
  methods: {
    reversedMessageMethod: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    },
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
    }
  }
}
</script>

<style lang="less" scoped>
  @import "./Class.less";
</style>
