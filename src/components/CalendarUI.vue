<template>
  <DatePicker
    v-model="date"
    :attributes="attrs"
    :select-attribute="selectAttribute" />
</template>

<script>
import { DatePicker} from 'v-calendar'

export default {
  name: 'CalendarUI',
  components: {
    DatePicker,
  },
  data() {
    return {
      attrs: [
        { ket: 'today',
        highlight: {
          color: 'green',
          fillMode: 'outline',
        },
        dates: new Date()
        }
      ],
      selectAttribute: {
        highlight: 'green',
      },
       date: new Date(),
    }
  }
}

</script>
