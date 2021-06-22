<template>

</template>

<script>
export default {
  data () {
    return {
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      }
    }
  },
  methods: {
    closeDetail () {
      this.preordervisable = false
    },
    onSubmit () {
      console.log('submit!')
    }
  }
}
</script>

<style scoped>

</style>
