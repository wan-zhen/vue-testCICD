<template>
  <div>
    <demo @update-msg="update"></demo>
    {{num}}
    <HelloWorld msg="test1"/>
    <hello-world msg="test2"></hello-world>
  </div>
</template>

<script>
// @ is an alias to /src
import Demo from '@/components/Demo.vue';
import HelloWorld from '@/components/HelloWorld.vue';

export default {
  name: 'about',
  components: {
    Demo,
    HelloWorld
  },
  data: function() {
    return {
      num: 0
    };
  },
  methods: {
    update: function(inputdata) {
      console.log(inputdata);
      this.num += 1;
    }
  },
  created: function() {
    console.warn('about', this);
  }
};
</script>
