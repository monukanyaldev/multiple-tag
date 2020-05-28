# my-vue-library

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

## Installation and Usage
1. Just use `npm install --save @monukanyal/multiple-tag`
2. next step:
``` App.vue
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
```
3. use like this:
```
 <tag-input v-on:getInputs="getEmailInputs"></tag-input>
```