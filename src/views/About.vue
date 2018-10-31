<template>
  <div>
    <demo></demo>
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
  }
};
</script>
