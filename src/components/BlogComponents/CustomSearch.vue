<template>
  <input type="text" class="py-1 px-2" v-model="inputValue" @input="onInput" />
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'CustomSearch',
  props: {value: String},
  data: ()=> ({
    inputValue: ''
  }),
  computed:{
    
  },
  methods: {
    onInput(){
      this.$emit('input', this.inputValue);
    }
  },
  watch: {
    value() {
      this.inputValue = this.value;
    }
  },
  filters: {
  }
}
</script>

<style scoped lang="scss"></style>