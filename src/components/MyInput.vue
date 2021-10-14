<template>

  <input :value="modelValue" @input='$emit("update:modelValue", $event.target.value)'/>
  
</template>

<script>
export default {
  name: 'Myinput',
  props: ['modelValue'],
  methods: {
   
},
}



</script>

<style>

</style>