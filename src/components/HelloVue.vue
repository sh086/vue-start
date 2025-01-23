<template>
  <div class="hello-vue">
    <span><input type="text" v-model="name" placeholder="请输入您的姓名"></input></span>
    <p v-if="name">欢迎 {{name}} 开启Vue3学习之旅!</p>
  </div>
</template>

<script setup lang="ts" name ="HelloWord">
    import {ref} from 'vue'
    let name = ref("Vue3")
</script>

<style scoped>
.hello-vue {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: 'Arial', sans-serif;
  text-align: center;
  margin: 0 auto;
  padding: 20px;
  width: 300px;
  border: 2px solid #4CAF50;
  border-radius: 10px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  background-color: #f9f9f9;
}
</style>

<!-- 
知识点：
============================================
目录
1、setup语法糖
2、设置组件名
3、数据双向绑定
============================================
1、setup语法糖
① setup语法糖可以简化代码
② setup中的this是undefined,vue3中不建议this
③ setup是在data和methods和之前加载的,所以data和methods可以调用setup的数据,但是setup不能使用data的数据

// 示例
<script setup lang="ts" name ="HelloWord">
    import {ref} from 'vue'
    let name = ref("Vue3")
</script>

上述使用原版语法是这样实现的：
<script lang="ts">
  import {ref} from 'vue'
  export default {
    name: 'HelloVue',  // 组件名称
    setup() {
        let name = ref("Vue3")
        // 对外暴露数据与方法,使用setup语法糖可以简化此处
        return {
          name
      }
    }
  }
</script>
============================================

2、设置组件名
方案一：可在一个单独的script中定义
  // 单独用于设定组件名称
  <script lang="ts">
    export default {
        name: 'Person',  // 组件名称
    }
  </script>
  // 用于业务逻辑
  <script setup lang="ts" name ="HelloWord">
    ...
  </script>
方案二：按照插件支持
  ① npm i vite-plugin-vue-setup-extend -D
  ② vite.config.ts 补充
  import vueSetUpExtend from 'vite-plugin-vue-setup-extend'
  export default defineConfig({
    plugins: [
      vueSetUpExtend(),
    ],
    ...
  })
  ③ 这样就可以在脚本中使用name设定组件名了
  <script setup lang="ts" name ="Person"></script>

============================================
3、数据双向绑定
// 使用之前需要导入
import {ref} from 'vue'

① ref定义基本类型和对象类型的响应式数据
// 使用ref绑定基本类型响应式数据
let name = ref("")
name.value += 1

// 使用ref绑定对象类型的响应式数据
let person = ref({firstName:'张',lastName:"三",fullName:"张三",age:18,tel:'123456789'})
person.age.value += 1
说明：可以开启自动补全.value

② reactive定义对象类型的响应式数据
let person = reactive({firstName:'张',lastName:"三",fullName:"张三",age:18,tel:'123456789'})
person.age += 1
============================================

-->