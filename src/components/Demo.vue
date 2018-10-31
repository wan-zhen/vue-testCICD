<template>
  <div>
    <input v-model="msg">
    <p>{{msg}}</p>
    <p>{{reversedMessageComputed}}</p>
    <p>{{reversedMessageMethods()}}</p>
    
    <p v-html="html"></p>
    <p v-if="seen">
      看的到ㄏㄏ看不到ㄎㄎ
      <input placeholder="true">
    </p>
    <p v-else>
      看的到ㄎㄎ看不到ㄏㄏ
      <!-- 用 v-else 相同的元素會被重複使用 所以在切換時 input 的值會被留著 -->
       <input placeholder="false">
    </p>
        <p v-if="seen">
      看的到ㄏㄏ看不到ㄎㄎ
      <input placeholder="true" key="demo1">
    </p>
    <p v-else>
      看的到ㄎㄎ看不到ㄏㄏ
      <!-- 用 v-else 相同的元素會被重複使用 所以在切換時 input 的值會被留著 給獨立的 key 讓一樣的 element 不要被複用 -->
       <input placeholder="false" key="demo2">
    </p>
    <button :disabled="false" @click="changeData">Button</button>
  </div>
</template>

<script>
export default {
  name: 'Demo',
  data: function() {
    return {
      msg: 'demo',
      html: '<span style="color:red;">red</span>',
      seen: false
    };
  },
  // 响应式依赖 计算属性是基于它们的依赖进行缓存的 (cache)
  // 依賴的值沒有改變 放在這裡的屬性不會走進來
  // ex : this.msg 有被改變才會走進來 如果想在畫面上一直走進來 除非改動 msg 的值，
  //      不然就改用 method 綁在畫面上吧
  computed: {
    // 计算属性的 getter
    reversedMessageComputed: function() {
      console.log('computed');
      // `this` 指向实例
      return this.msg
        .split('')
        .reverse()
        .join('');
    }
  },
  // 跟 computed 不同的點只有在於 只要有呼叫到都會進來
  // 不像 computed 有 cache 依賴的值就不會再走進來
  methods: {
    changeData: function() {
      this.seen = !this.seen;
      this.msg = '你好';
    },
    // 也可直接綁在表達式上
    reversedMessageMethods: function() {
      console.log('methods');
      // `this` 指向实例
      return this.msg
        .split('')
        .reverse()
        .join('');
    }
  },
  // computed 加強版 当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的
  watch: {
    // 如果 `msg` 发生改变，这个函数就会运行
    msg: function(newQuestion, oldQuestion) {
      console.log(newQuestion, oldQuestion);
    }
  },
  beforeCreate: function() {
    console.warn('beforeCreate', this.msg);
  },
  created: function() {
    console.warn('created', this.msg);
  },
  beforeMount: function() {
    console.warn('beforeMount');
  },
  mounted: function() {
    console.warn('mouted');
  },
  beforeUpdate: function() {
    // 有值被改變
    console.warn('beforeUpdate');
  },
  updated: function() {
    console.warn('updated');
  },
  beforeDestroy: function() {
    console.warn('beforeDestroy');
  },
  destroyed: function() {
    console.warn('destroyed');
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
