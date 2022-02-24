<template>
  <div class="todo">
    <div class="todo--head">
      <strong>TODO</strong>
      <AddBtn
        @click="showToggle" />
      <EditBtn 
        @click="editToggle" />
      <DeleteBtn
        @click="deleteToggle" />
    </div>
    <GetTitle
      v-show="isShow"
      @add_todo="askReload" />
    <List
      :todos="todoYet"
      :delete="this.delete"
      :edit="edit"
      @reload="askReload" />
  </div>
</template>

<script>
import AddBtn from '~/components/AddBtn'
import EditBtn from '~/components/EditBtn'
import DeleteBtn from '~/components/DeleteBtn'
import List from '~/components/List'
import GetTitle from '~/components/GetTitle'

export default {
    components: {
    AddBtn,
    EditBtn,
    DeleteBtn,
    List,
    GetTitle
  },
  props: {
      todoYet: {
        type: Array,
        default: () => []
      }
  },
  emits: ['reload'],
  data(){
    return {
      todo: '',
      isShow: false,
      delete: false,
      edit: false,
    }
  },

  methods:{
    showToggle(){
         this.isShow = !this.isShow
         this.edit = false
         this.delete = false
         console.log(this.isShow)
    },
    deleteToggle(){
         this.delete = !this.delete
         this.edit = false
         console.log(this.delete)
    },
    editToggle(){
         this.edit = !this.edit
         this.delete = false
         console.log(this.edit)
    },
    askReload(){
      this.$emit('reload')
    }
  }
}
</script>
