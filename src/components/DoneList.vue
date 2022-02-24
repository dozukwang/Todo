/<template>
  <div class="done">
    <div class="done--head">
      <strong>DONE</strong>
      <DeleteBtn 
        @click="deleteToggle" />
    </div>
    <List
      :todos="todoDone"
      :delete="this.delete"
      @reload="askReload" />
  </div>
</template>

<script>
import DeleteBtn from '~/components/DeleteBtn'
import List from '~/components/List'

export default {
  components: {
    DeleteBtn,
    List,
  },
    props: {
      todoDone: {
        type: Array,
        default: () => []
      }
  },
  emits: ['reload'],
  data(){
    return {
      delete: false,
    }
  },

  methods:{

    deleteToggle(){
         this.delete = !this.delete
         this.edit = false
         console.log(this.delete)
    },
    askReload(){
      this.$emit('reload')
    }
  }
}
</script>
