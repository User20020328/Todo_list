<script setup>
import { ref } from 'vue';
import axios from 'axios';


const value = ref('')
const list = ref([])

async function getList(){
const res = await axios({
  url:'https://jmsjte43vf.sealoshzh.site/get_list',
  method:'GET',
})
  list.value = res.data.list
}


async function add(){
  await axios({
    url:'https://jmsjte43vf.sealoshzh.site/add_todo',
    method:'POST',
    data:{
      value: value.value,
      isComplete:false,

    },
  })
  console.log('当前输入框的值是：', value.value) 
  getList()

  value.value =''
}


async function update(id){
  await axios({
    url:'https://jmsjte43vf.sealoshzh.site/update_todo',
    method:'POST',
    data:{
      id,
    },
  })

  getList()
}

async function del(id) {
  await axios({
    url:'https://jmsjte43vf.sealoshzh.site/del_todo',
    method:'POST',
    data:{
      id:id ,
    }
  })

  getList()
}


</script>

<template>
    <div class="todo-app">
        <div class="title">Todo App</div>
        <div class="todo-form">
            <input v-model="value" class="todo-input" type="text" placeholder="Add a todo"/>
            <div @click="add" class="todo-botton">Add Todo</div>
        </div>

        
        <div v-for="(item,index) in list" :class="[item.isComplete ? 'completed':'item']">
            <div>
                <!-- 可打勾的小框 -->
                <input v-model="item.isComplete" type="checkbox"/>
                <!-- 具体代办文字 -->
                <span class="name"> {{item.value}} </span>
            </div>
            <div @click="del(item._id)" class="del">del</div>

        </div>

        
    </div>

</template>

    <style>
        body {
            background: linear-gradient(
                to right,
                rgb(218,235,238),
                rgb(182,215,227)
            );
        }

        .todo-app{
            width: 98%;
            height: auto;
            padding-top: 30px;
            /* 加入pading后，让框的高度仍保持500px */
            box-sizing: border-box; 
            background-color: #fffafa;
            border-radius: 12px;
            margin-top: 40px;
            margin-left: 1%;
        }

        .title{
            font-size: 30px;
            font-weight: 700;
            text-align: center;
            font-family:Verdana, Geneva, sans-serif;
        }

        /* 让输入框与按钮在同一行 */
        .todo-form{
            display: flex;
            margin-left: 30PX;
            margin-top: 20px;
            justify-content: center;
        }

        .todo-input{
            padding-left: 15px;
            border: 1px solid #dfe1e5;
            outline: no;
            width: 60%;
            height: 50px;
            border-radius: 20px 0 0 20px;
        }
        .todo-botton{
            width: 100px;
            height: 55px;
            border-radius: 0 20px 20px 0;
            text-align: center;
            background-color: #FF8C00;
            /* 不能选取字体复制 */
            user-select: none;
            /* 接近按钮鼠标变成手指 */
            cursor: pointer;
            /* 字体上下居中 */
            line-height: 52px;
            /* 字体颜色 */
            color: #1C1C1C
        }
        
        .item{
            display: flex;
            align-items: center;
            box-sizing: border-box;
            width: 80%;
            height: 50px;
            margin: 20px auto;
            padding: 16px;
            border-radius: 20px;
            box-shadow: rgba(149,157,165, 0.2) 0px 8px 20px;
            /* 让“勾选框+文字”与“del”平分框的空间 */
            justify-content: space-between;
        }

        .del{
            color: brown;
        }

        .completed{
          display: flex;
            align-items: center;
            box-sizing: border-box;
            width: 80%;
            height: 50px;
            margin: 20px auto;
            padding: 16px;
            border-radius: 20px;
            box-shadow: rgba(149,157,165, 0.2) 0px 8px 20px;
            /* 让“勾选框+文字”与“del”平分框的空间 */
            justify-content: space-between;
            text-decoration: line-through;
            opacity: 0.4;
        }
    </style>
