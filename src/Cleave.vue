<template>
  <input ref="input" type="text" v-bind:value="value" @input="updateValue" />
</template>

<script>
import Cleave from 'cleave.js'

export default {
  props: {
    value: {
      type: String,
      default: ''
    },
    options: {
      type: Object,
      default: () => ({})
    },
    events: {
      type: Object,
      default: () => ({})
    }
  },

  data () {
    return {
      cleave: null
    }
  },

  methods: {
    updateValue() {
      const vm = this
      setTimeout(function(){
        vm.$emit('input', vm.$el.value)
      }, 0)
    },
  },

  mounted () {
    this.cleave = new Cleave(this.$el, this.options)
    Object.keys(this.events).map((key) => {
        this.$refs.input.addEventListener(key, this.events[key])
    })
  },

  beforeDestroy () {
    this.cleave.destroy()
  },

  watch: {
      options: {
          deep: true,
          handler(val) {
              this.cleave.destroy()
              this.cleave = new Cleave(this.$el, val)
          },
      }
  },

}
</script>
