<template>
  <div class="list--container">
    <ul>
      <template
        v-for="todo in todos"
        :key="todo.id">
        <li class="list">
          <form 
            v-if="editNow && todo.id === editIDCheck"
            @submit.prevent="editTitle($event.target[0].value, todo)">
            <input
              type="text" />
          </form>
          <span
            v-else
            class="list__title">{{ todo.title }}</span>
          <div class="edit_time">
            <img src="../../static/watch.svg" /> 수정시간: <span class="time"> {{ todo.updatedAt }}</span>
          </div>
          <button
            v-if="!this.delete && !edit && !todo.done"
            class="check_btn"
            @click="completeItem(todo)">
            완료
          </button>
          <button
            v-if="!this.delete && !edit && todo.done"
            class="check_btn"
            @click="completeItem(todo)">
            취소
          </button>
          <button
            v-if="this.delete && !edit"
            class="mode_delete"
            @click="deleteItem(todo.id)">
            삭제
          </button>
          <button
            v-if="edit && !this.delete"
            class="mode_edit"
            @click="editItem(todo)">
            수정
          </button>
        </li>
      </template>
    </ul>
  </div>
</template>

<script>
  import axios from 'axios'
  import { BASE_URL } from '~/utils/const'

export default {
  props: {
    todos: {
      type: Array,
      default: () => []
    },
    delete: {
      type: Boolean,
      default: false
    },
    edit: {
      type: Boolean,
      default: false
    },
    reload: {
      type: Boolean,
      default: false,
    }
  },
    emits: ['reload'],
  data(){
    return{
      idDelete: [],
      editNow: false,
      editIDCheck: ''
    }
  },
  
  methods:{
    async completeItem(todo){
      await axios({
        url: `${BASE_URL}/${todo.id}`,
        method: 'PUT',
        headers: {
        'content-type': 'application/json',
        'apikey': process.env.API_KEY,
        'username': process.env.USERNAME
      },
        data: {
          'title': todo.title,
          'done': !todo.done,
          'order': 0
        }
      })
      console.log(todo.done)
      this.$emit('reload')
    },
    async deleteItem(id){
      console.log(id)
      await axios({
        method: 'DELETE',
        url: `${BASE_URL}/${id}`,
        headers: {
        'content-type': 'application/json',
        'apikey': process.env.API_KEY,
        'username': process.env.USERNAME
        },
      })
        console.log('deleted!')
        this.$emit('reload')
    },
    editItem(todo){
      this.editNow = !this.editNow
      this.editIDCheck = todo.id
    },
    async editTitle(newTitle, todo){
      await axios({
        url: `${BASE_URL}/${todo.id}`,
        method: 'PUT',
        headers: {
        'content-type': 'application/json',
        'apikey': process.env.API_KEY,
        'username': process.env.USERNAME
      },
        data: {
          'title': newTitle,
          'done': todo.done,
          'order': 0
        }
      })
        this.$emit('reload')
        this.editNow = false
    }
  }
}
</script>

<style lang="scss" scoped>
    @mixin btn($color){
    background-color: $color;
    width: 50px;
    height: 20px;
    padding: 0;
    font-weight: 500;
    border-radius: 5px;
    border: none;
    margin-left: 8px;
    }

  .list--container{
    background-color: #e4e4e4;
    box-shadow: rgba(9, 30, 66, 0.25) 0px 4px 8px -2px, rgba(9, 30, 66, 0.08) 0px 0px 0px 1.4px;
    border-radius: 8px;
    overflow-y: scroll;
    -ms-overflow-style: none; /* IE and Edge */
    scrollbar-width: none; /* Firefox */
    &::-webkit-scrollbar {display: none; /* Chrome, Safari, Opera*/}
    ul{
      padding: 10px;
      height: 600px;
    }
  }
  .list{
    background-color:#fff;
    height: 30px;
    position: relative;
    padding: 0 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-radius: 4px;
    box-shadow: rgba(0, 0, 0, 0.05) 0px 6px 24px 0px, rgba(0, 0, 0, 0.08) 0px 0px 0px 1px;
    .list__title{
      flex: 1;
      margin-right: 30px;
      overflow: hidden;
    }
    .edit_time{
      position: absolute;
      right: 6px;
      bottom: -22px;
      font-size: 14px;
      img{
        width: 18px;
        height: 18px;
        vertical-align: middle;
      }
    }
  }
  .list + .list{
    margin-top: 35px;
  }

  .check_btn{
   @include btn(#CFD8DC);
  }

  .mode_delete{
   @include btn(#f44336);
  }

  .mode_edit{
   @include btn(#ffd600);
  }
</style>
