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
      <!-- 也可以在 if else 設定 key 裡面的 element 全都不會被複用 會整個重繪 -->
       <input placeholder="false" key="demo2">
    </p>
    <button :disabled="false" @click="changeData">Button</button>
    <!-- 給唯一的 :key  類似 track-by  以便它能跟踪每个节点的身份，从而重用和重新排序现有元素 -->
    <ul>
      <li v-for="(v,i) in demoItems" :key="i">
        {{i}} - {{v.data}}
      </li>
    </ul>
    <!-- in of 都可 -->
    <ul>
      <li v-for="(v,i) of demoItems" :key="i">
        {{i}} - {{v.data}}
      </li>
    </ul>

    <button v-on:click="eventTest">Get Event</button>
    <button v-on:click="methodTest('Demo',$event)">Demo Method</button>
    <button v-on:click="methodTest('Test',$event)">Test Method</button>
    <br>
    <a v-on:click.prevent.self="methodTest('Test',$event)" href="">
      點擊不要被元素默認行為影響，会阻止所有的点击
      <a>不會有行為</a>
    </a>
    <br>
    <a @click.self.prevent="methodTest('Test',$event)" href="">
      點擊不要被元素默認行為影響，只会阻止对元素自身的点击
      <a>會有行為</a>
    </a>
    <br>
    <a @click.once.self.prevent="methodTest('Test',$event)" href="">点击事件将只会触发一次 包含prevent</a>
    <br>
    <input @keyup.enter="methodTest('Test',$event)">
    <br>
    <button @click="$emit('update-msg','test')">通知爸爸的method</button>
    <br>
    <button @click="show = !show">
      Toggle render
    </button>
    <transition name="slide-fade">
      <p v-if="show">hello</p>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'Demo',
  data: function() {
    return {
      msg: 'demo',
      html: '<span style="color:red;">red</span>',
      seen: false,
      demoItems: [{ data: 'a' }, { data: 'b' }, { data: 'c' }],
      show: true
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
    },
    eventTest: function(event) {
      console.log(event.target);
    },
    methodTest: function(msg, event) {
      console.log(msg);
      console.log(event);
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
    // instance 被 constructor 建立前
    console.warn('beforeCreate', this.msg);
  },
  created: function() {
    // 實體建立完成(instance 被 constructor 建立後)。資料 $data 已可取得，但 $el 屬性還未被建立
    console.warn('created', this.msg);
  },
  beforeMount: function() {
    // 執行元素掛載之前。 (綁定 DOM 之前)
    console.warn('beforeMount');
  },
  mounted: function() {
    // 元素已掛載， $el 被建立。
    // 如果你想改變 DOM 或資料，可以在這裡做，因為它還沒被瀏覽器「畫」出來，必須直到 updated 執行時，才會把最終的結果畫在瀏覽器上被看到。
    console.log(this.$el);
    console.warn('mouted');
  },
  beforeUpdate: function() {
    // 當資料變化時被呼叫，還不會描繪 View。 (資料更新，但尚未更新 DOM)
    console.warn('beforeUpdate');
  },
  updated: function() {
    // DOM 的更新已經完成，View 被顯示在畫面上。 (因資料更新，而更新 DOM)
    console.warn('updated');
  },
  beforeDestroy: function() {
    // 實體還可使用。 ( 移除 instance 之前 )
    console.warn('beforeDestroy');
  },
  destroyed: function() {
    // 實體銷毀。所有綁定被解除、事件偵聽被移除、子實體也被銷毀。 ( 移除 instance 之後 )
    console.warn('destroyed');
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* 可以设置不同的进入和离开动画 */
/* 设置持续时间和动画函数 */
.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active for below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>
