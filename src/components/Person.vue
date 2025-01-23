<template>
  <div class="person">
    <span>计数器：{{number}}</span><button @click="changeNumber">点我自增</button>
    <h2>---个人信息--</h2>
    <h4>
      姓：<input type="text" v-model="person.firstName">
      名：<input type="text" v-model="person.lastName">
    </h4>
    <h4>
      年龄：<input type="text" v-model="person.age">
      {{ageTip}}
    </h4>
    <h4>电话：{{person.tel}}</h4>
    <button @click="resetInitStudent">重置为张三</button>

    <h2>---预览信息--</h2>
    <h8>姓名：{{fullName}}</h8>
    <br><br><span>爱好游戏列表：</span>
    <ul>
      <!-- :key 是 v-band:key 的简写 -->
      <li v-for="game in games" :key="game.id">
        第{{game.id}}个 - {{game.name}}
      </li>
    </ul>
    <button @click="changeFirstGameName">修改第一个游戏名称</button>
  </div>
</template>

<script setup lang="ts" name ="Person">
  import {ref,reactive, computed,toRefs, watch} from 'vue'

  let number = ref(2)
  let ageTip = "";
  let person = reactive({firstName:'张',lastName:"三",fullName:"张三",age:18,tel:'123456789'})

  let games = reactive([
    {id:1,name:"原神"},
    {id:2,name:"lol"}
  ])
  

  let fullName = computed(()=>{
    person.fullName = person.firstName + person.lastName
    return person.fullName
  })
  // let {name,age,tel} = toRefs(person)

  // 方法
  function changeNumber(){
    number.value += 1
  }

  function changeFirstGameName(){
    games[0].name = '王者'
  }

  //监视person.age存在变化
  watch(()=>person.age,()=>{
    console.log('person.age变化了')
    if(person.age > 18){
      ageTip = "输入的年龄大于18"
    }
  })


  // 监控整个person以及属性存在变化
  
  // watch(()=>person,()=>{
  //   if(person.age > 18){
  //     ageTip = "输入的年龄大于118"
  //   }
  // },{deep:true})

  // watch(()=>person,(value) 只取新值

    // 监控整个person的变化
    // 第一个参数：被监视的对象
    // 第二个参数：监视的回调函数
    // 第三个参数：配置对象 deep 和 immediate
    // watch(()=>person,(newValue,oldValue)=>{
    // if(person.age > 18){
    //   ageTip = "输入的年龄大于118"
    // }
    // })

    // 若被监视的是reactive定义的对象类型，则默认是开启深度监听，且是无法被关闭

    // 监视响应式对象中某个属性，建议写成函数式 若需要深度监视则deep:true
    // 若是对象 可以也建议写出函数式
    // 监视的是person.car的地址，若该对象发生变化
    // watch(()=>person.car,(newValue,oldValue)=>{
    //   if(person.age > 18){
    //     ageTip = "输入的年龄大于18"
    //   }
    // })

    // 监视的是person.car指向对象的地址，若该指向对象发生变化这会被监视到 但是 若xxx
    // watch(person.car,(newValue,oldValue)=>{
    //   if(person.age > 18){
    //     ageTip = "输入的年龄大于18"
    //   }
    // })

  

  function resetInitStudent(){
    // 若person使用reactive进行数据绑定 只能通过Object.assign重新赋值
    Object.assign(person,{firstName:'张',lastName:"三",fullName:"张三",age:18,tel:'123456789'})
    // 若person使用ref进行数据绑定 则可以通过.value整体赋值 
    //person.value = {name:'王五',age:20,tel:'987654321'}
  }

</script>

<!-- 
知识点：
============================================
目录
1、
============================================


============================================

============================================
-->

<!-- 总结：


(5)转换
  toRefs：将reactive定义的响应式对象的每一组key-value改为由ref定义的响应式数据
  let {name,age,tel} = toRefs(person)
  let n1 = toRef(person,'name')
  使用person.mame 和直接使用 name是一个效果 都是双向绑定的

  (5)计算属性
    // let firstName = ref('张')
  // let lastName = ref('三')
  // // 只有依赖的数据发生变化 fullName就会自动重新计算
  // // 在多次调用时，只会执行一次computed的函数
  // // 定义的fullName是一个计算属性 且是只读的
  // // let fullName = computed(()=>{
  // //   return firstName.value + lastName.value
  // // })

  // 这样定义的fullName是一个计算属性 是可读可写的
  let fullName = computed({
    // fullName展示时调用
    get(){
      return person.firstName + person.lastName
    },
    // fullName修改时调用
    set(val){
      //const [str1,str2] = val.split('-')
      person.firstName = val.slice(0,1)
      person.lastName = val.slice(1,val.length)
    }
  })

  （6）修改整个数据
   // 若person使用reactive进行数据绑定 只能通过Object.assign重新赋值
   这个是将原来的对象重新赋值了
    Object.assign(person,{firstName:'张',lastName:"三",fullName:"张三",age:18,tel:'123456789',car:{id:"",name:""}})
    等价于 person.firstName = '张' person.car = {}
    // 若person使用ref进行数据绑定 则可以通过.value整体赋值 
    // 这种写法真正的是用一个新对象替换了原有对象
    //person.value = {name:'王五',age:20,tel:'987654321'}

-->



<style>
  .person {
    background-color: skyblue;
    box-shadow: 0 0  10px;
    border-radius: 10px;
    padding: 20px;
  }
  button {
    margin: 0 10px;
  }

</style>