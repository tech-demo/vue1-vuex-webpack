<template>
  <div>
    <h1>Vue, Vue router 学习要点</h1>
    <li>vue 基本属性</li>
    <li>vue 网络请求</li>
    <li>vue 生命周期</li>
    <li>vue 组件</li>
    <li>vue router 定义以及页面之间传值</li>
    <li>vue 与 async</li>
    <h1>{{ msg }}</h1>
    <zn-data :data="data" ></zn-data>
    <a v-link="{ name: 'i18n', params: { id: 123 }}">i18n - v-link</a>
    <button @click="goPage2">i18n - button</button>
    <button @click="changeMsgAsync">async 更改文字</button>
  </div>
</template>

<script>
import Thread from 'plugins/thread';

export default {
  el: '',
  props: ['msg2'],
  data() {
    return {
      msg: '等待 3s async ready 会改变文字值',
      data: null,
    }
  },
  methods: {
    async goPage2() {

      // 下面 2 种方式是相同的, 无论哪种,都只能通过 url 传参数
      // https://github.com/vuejs/vue-router/blob/v0.7.11/docs/zh-cn/api/go.md
      // this.$router.go({
      //   path: '/i18n/cc',
      //   query: { a: 1 },
      // });
      this.$router.go({
        name: 'i18n',
        query: { a: 1 },
        params: { id: 'cc' }
      });
    },
    async changeMsgAsync() {
      // 测试 method 中 async 写法是可行的
      this.data = await this.$http.get('/smm-2017-01-03.json');
      console.warn('this.$http: ', this.data)
    }
  },
  computed: {},
  events: {},

  // 不建议使用 async, 即使使用了也不会 block 正常流程
  created() {
    console.warn('1. created before sleep')
    Thread.sleep(1000)
    console.warn('1. created after sleep')
  },

  // 不建议使用 async, 即使使用了也不会 block 正常流程
  beforeCompile() {
    console.warn('2. beforeCompile before sleep')
    Thread.sleep(1000)
    console.warn('2. beforeCompile after sleep')
  },

  // 不建议使用 async, 即使使用了也不会 block 正常流程
  compiled() {
    console.warn('3. compiled before sleep')
    Thread.sleep(1000)
    console.warn('3. compiled after sleep')
  },

  async ready() {
    console.warn('4. ready before sleep')
    await Thread.sleep(1000)
    this.msg = 'async finished';
    console.warn('4. ready after sleep')
  },

  // 不建议使用 async, 即使使用了也不会 block 正常流程
  beforeDestroy() {
    console.warn('5. beforeDestroy before sleep')
    Thread.sleep(1000)
    console.warn('5. beforeDestroy after sleep')
  },

  async destroyed() {
    console.warn('6. destroyed before sleep')
    await Thread.sleep(3000)
    console.warn('6. destroyed after sleep')
  }
}
</script>

<style lang="less" scoped>
body {
  font-family: Helvetica, sans-serif;
}

button,
a {
  display: block;
}
</style>
