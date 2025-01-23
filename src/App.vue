<template>
  <!-- <HelloVue/> -->
  <!-- <GuessNumber/> -->
  <Person/>
  <!-- <FormSubmit/> -->
  <!-- <StudentSys/> -->
</template>

<script lang="ts"> 
  import Person from "./components/Person.vue"
  import HelloVue from "./components/HelloVue.vue"
  import GuessNumber from "./components/GuessNumber.vue"

  export default {
    name: 'App' , // 组件名
    components: { // 注册组件
      HelloVue,
      Person,
      GuessNumber,
    }
  }

</script>