<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" height="100px">
    <div class="container">
      <h2>Multiple E-mail separated with comma</h2>
     <tag-input v-on:getInputs="getEmailInputs"></tag-input>
      <ul id="example-1">
        <li v-for="(item,index) in inputs.emails" :key="index">
          {{ item }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import TagInput from './components/TagInput.vue'

export default {
  name: 'App',
  components: {
    'tag-input':TagInput
  },
  data () {
    return {
      inputs:{
        emails: [],
        invalid: false
      } 
    }
  },
  methods: {
    getEmailInputs(inputs) {
      this.inputs = inputs
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  display: block;
  width: 50%;
  margin-left: 25%;
}
</style>
