<template>
  <v-container fluid>
    <h4>Add, Remove and Edit Items in nested Value-Array</h4>

    <v-form-base
      id="array"
      :value="myValue"
      :schema="mySchema"
      @change:array="change"
    />

    <v-btn
      dark
      color="blue"
      @click="add"
    >
      <v-icon>mdi-plus</v-icon>
    </v-btn>

    <infoline
      :value="myValue"
      :schema="mySchema"
      :path="$options._componentTag"
    />
  </v-container>
</template>

<script>
import VFormBase from '@/components/vFormBase'
import Infoline from '@/components/infoline'
import update from '@/lib'

export default {
  name: 'Array',
  components: { VFormBase, Infoline },
  data () {
    return {
      myValue: {
        tasks: [
          {
            add: '',
            nr: 1,
            task: 'coding',
            done: [
              this.getTicket(),
              this.getTicket(),
            ]
          },
          {
            add: '',
            nr: 2,
            task: 'Work out',
            done: [
              this.getTicket(),
              this.getTicket(),
              this.getTicket(),
            ]
          }
        ]
      },
      mySchema: {
        tasks: {
          type: 'array',
          flex: 12,
          schema: {
            add: { type: 'btn', 'small': true, iconCenter: 'add', dark: true, color: 'red', flex: 1 },
            nr: {
              type: 'text',
              disabled: true,
              color: 'blue',
              flex: 1
            },
            task: {
              type: 'text',
              color: 'blue',
              flex: 10
            },
            done: {
              type: 'array',
              schema: {
                done: {
                  type: 'checkbox', label: 'Done', color: 'red', offset: 1, flex: 2 },
                  title: { type: 'text', color: 'red' },
              }
            }
          }
        }
      }
    }
  },
  methods: {

    getTicket(){ return { done: false, title: 'Ticket added ' + Math.floor(Math.random() * 1000) } },

    add () {
      this.myValue.tasks.push({
        add: '',
        nr: this.myValue.tasks.length + 1,
        task: 'item ' + (this.myValue.tasks.length + 1) + ' added',
        done: [
          this.getTicket()
        ]
      })
    },

    change (val) {
      let { on, id, index, key, value } = update(val)

      // add task
      if (key === 'add') {
        setTimeout(() => this.myValue.tasks[index[0]].done.push( this.getTicket() ), 250)
      }
      // remove task
      if (key === 'done' && value === true) {
        setTimeout(() => this.myValue.tasks[index[0]].done.splice(index[1], 1), 250)
      }
    }
  }
}
</script>
