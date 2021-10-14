<template>
 
    <select :value="modelValue" @change="selected">
        <option disabled value="">Choose option</option>
        <option  v-for="option in options" :key="option.id" :value="option.id">{{option.title}}</option>
    </select>
</template>

<script>
export default {
    name: 'my-select',
    props: {
      modelValue: {
        required: false,
      },
      options: {
        required: false,
      }
    },
    methods: {
      selected(e){
        // console.log(e.target.value)
        this.$emit('update:modelValue', e.target.value)
      }
    },
    
}

</script>

<style scoped>



</style>