<template>
  <div>
    <span>새로운 할 일</span>
    <form
      :value="modelValue"
      @submit.prevent="$emit('update:modelValue', $event.target[0].value)">
      <input
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

export default {
  props: {
    modelValue: {
      type: String,
      default: ''
  },
},
emits: ['update:modelValue'],
}
</script>

<style lang="scss" scoped>
  div{
    border: 1px solid black;
    margin: 20px;
    padding: 20px;
  }
</style>
