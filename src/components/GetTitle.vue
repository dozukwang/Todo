<template>
  <div>
    <span>새로운 할 일</span>
    <form
      @submit.prevent="createTodo($event.target[0].value), clearInput()">
      <input
        class="todo__title"
        type="text"
        placeholder="할 일을 입력하세요." />
      <button
        type="submit"
        aria-label="추가하기">
        저장
      </button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'
import { BASE_URL } from '~/utils/const'

export default {
  emits: ['add_todo'],
  data(){
    return{
    }
  },
  methods:{
    async createTodo(title) {
      const { data } = await axios({
      url: BASE_URL,
      method: 'POST',
      headers: {
        'content-type': 'application/json',
        'apikey': process.env.API_KEY,
        'username': process.env.USERNAME
      },
      data: {
        'title': title,
        'order': ''
      }
    })
    this.$emit('add_todo', data)
    },
    clearInput(){
      const inputValue = document.querySelector('.todo__title')
      inputValue.value = ''
    }
  }
}
</script>

<style lang="scss" scoped>
  div{
    margin: 10px 0;
    padding: 20px;
    border-radius: 10px;
    background-color: rgba(255,255,255,.7);
    box-shadow: rgba(9, 30, 66, 0.25) 0px 4px 8px -2px, rgba(9, 30, 66, 0.08) 0px 0px 0px 1.4px;
    font-weight: 500;
  }
  form{
    margin-top: 12px;
    display: flex;
    align-items: center;
    .todo__title{
      flex: 1;
      width:100%;
      padding: 3px 5px;
    }
    button{
    background-color: #17307C;
    width: 50px;
    height: 25px;
    padding: 0;
    font-weight: 500;
    border-radius: 5px;
    border: none;
    margin-left: 8px;
     color: #fff;
    }
  }
</style>
