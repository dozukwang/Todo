<template>
  <div class="main_warp">
    <header>
      <img
        src="../static/event_available_white_24dp.svg"
        alt="logo" />
      <h3 class="page_title">
        할 일이 많을 때
      </h3>
    </header> 
    <main>
      <div class="calendar">
        <CalendarUI 
          class="calendar--container" />
      </div>
      <div class="todolist">
        <TodoList
          :todo-yet="todoYet"
          @reload="reloadTodo" />
        <DoneList
          :todo-done="todoDone"
          @reload="reloadTodo" />
      </div>
    </main>
  </div>
</template>

<script>
import CalendarUI from './components/CalendarUI'
import TodoList from './components/TodoList'
import DoneList from './components/DoneList'

import dayjs from 'dayjs'
import axios from 'axios'
import { BASE_URL } from '~/utils/const'

export default {
  components: {
    CalendarUI,
    TodoList,
    DoneList,
  },
  data(){
    return {
      todos: [],
      todoYet: [],
      todoDone: [],
      }
  },
  async created(){
    this.reloadTodo()
  },
  methods:{
    async reloadTodo() {
      console.log('reload')
      this.todos = await this.readTodo()
      this.todoYet = this.todos.filter(todo => todo.done === false)
      .sort((a, b) => {
          if (b.updatedAt > a.updatedAt) return -1
          else if (b.updatedAt < a.updatedAt) return 1
      })
      .map(todo => {
      todo.updatedAt = dayjs(todo.updatedAt).format('MM/DD h:mm A')
      return todo
      }
    )
      this.todoDone = this.todos.filter(todo => todo.done === true)
      .sort((a, b) => {
          if (b.updatedAt > a.updatedAt) return -1
          else if (b.updatedAt < a.updatedAt) return 1
      })
      .map(todo => {
      todo.updatedAt = dayjs(todo.updatedAt).format('MM/DD h:mm A')
      return todo
      }
    )
    },
    async readTodo() {
      const { data } = await axios({
        url: BASE_URL,
        method: 'GET',
        headers: {
          'content-type': 'application/json',
          'apikey': process.env.API_KEY,
          'username': process.env.USERNAME
        }
      })
      return data
    },
  }
}

</script>

<style lang="scss">
@mixin btn($color, $width, $right) {
    background-color: $color;
    width: $width;
    height: 30px;
    padding: 0;
    border-radius: 10px;
    border: none;
    position: absolute;
    right: $right;
    bottom: 9px;
    box-shadow: rgba(0, 0, 0, 0.16) 0px 1px 4px;
}

@mixin --head{
  font-size: 25px;
  font-weight: 700;
  position: relative;
  padding: 6px 0 10px;
& strong{
  margin-left: 16px;
  }
}

//COMMON
  html, #app, body{
    box-sizing: border-box;
    height: 100%;
  }
  body{
    background-color: #F1F8E9;
    font-size: 18px;
    font-family: 'Spoqa Han Sans Neo', 'sans-serif';
    line-height: 1;
}
  .add_btn{
    @include btn(#17307C, 100px, 130px);
  }
  .edit_btn{
    @include btn(#ffd600, 50px, 70px);
  }
  .delete_btn{
    @include btn(#f44336, 50px, 10px);
  }

//STYLE
  .main_warp{
    height: 100%;
    background-color: #DCEDC8;
    display: flex;
    flex-direction: column;
    overflow-y: scroll;
    -ms-overflow-style: none; /* IE and Edge */
    scrollbar-width: none; /* Firefox */
    &::-webkit-scrollbar {display: none; /* Chrome, Safari, Opera*/}
  }
  header{
    background-color: #FFF;
    padding: 10px 20px;
    margin-bottom: 20px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    & img{
      background-color: #1B5E20;
      padding: 2px;
      border-radius: 5px;
      width: 20px;
      height: 20px;
    }
    & .page_title{
    font-weight: 500;
    margin-left: 8px;
    margin-bottom: -1px;
    }
  }

  //MAIN
  main{
    flex: 1;
    display: flex;
    padding: 0 30px 30px;
    
    //CALENDAR
    .calendar{
      margin-right: 80px ;
      height: 270px;
      .calendar--container{
      box-shadow: rgba(9, 30, 66, 0.25) 0px 4px 8px -2px, rgba(9, 30, 66, 0.08) 0px 0px 0px 1.4px;
      }

    }
    .todolist{
      flex: 1;
      display: flex;
      min-width: 830px;
    }
  }

  //TODO
  .todo{
    flex: 1;
    min-width: 400px;
    .todo--head{
      @include --head;
    }
  }

  //DONE
  .done{
    flex: 1;
    min-width: 400px;
    margin-left: 30px;
    .done--head{
      @include --head;
    }
  }


</style>
