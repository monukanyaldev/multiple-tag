<template>
    <div class="box">
    <div class="tag-container" :class="{invalid:isInvalid}">
      <input @keyup="tagkeyUpChange"/>
    </div>
    </div>
</template>
<script>
import {email} from 'vuelidate/lib/validators'

export default {
  name: 'TagInput',
  data () {
    return {
      invalid: false,
      emails: [],
      removeItem: null,
      tagContainer: document.querySelector('.tag-container'),
      input: document.querySelector('.tag-container input')
    }
  },
  computed: {
    isInvalid () {
      return this.invalid
    }
  },
  watch: {
    emails: function (val) {
      this.$emit('getInputs', {emails: val, invalid: this.isInvalid})
    },
    invalid: function (val) {
      this.$emit('getInputs', {emails: this.emails, invalid: val})
    }
  },
  mounted () {
    this.tagContainer = document.querySelector('.tag-container')
    this.input = document.querySelector('.tag-container input')
    this.input.focus()
  },
  methods: {
    createTag (label) {
      const div = document.createElement('div')
      div.setAttribute('class', 'tag')
      const span = document.createElement('span')
      span.innerHTML = label
      const closeIcon = document.createElement('i')
      closeIcon.setAttribute('class', 'fa fa-times')
      closeIcon.setAttribute('arial-hidden', 'true')
      closeIcon.setAttribute('data-item', label)
      closeIcon.addEventListener('click', this.removeEmail)
      div.appendChild(span)
      div.appendChild(closeIcon)
      return div
    },

    clearTags () {
      document.querySelectorAll('.tag').forEach(tag => {
        tag.parentElement.removeChild(tag)
      })
    },

    addTags () {
      this.clearTags()
      this.emails.slice().reverse().forEach(tag => {
        this.tagContainer.prepend(this.createTag(tag))
      })
    },
    tagkeyUpChange (e) {
      this.validateCompleteInput(e)
      if (e.target.value !== '') {
        // (e.key === 'Enter')
        if (e.target.value.endsWith(',')) {
          this.invalid = false
          var emailInput = e.target.value.replace(',', '')
          if (!email(emailInput)) {
            this.invalid = true
            return false
          }
          emailInput.split(',').forEach(tag => {
            this.emails.push(tag)
          })

          this.addTags()
          this.input.value = ''
        }
      }
    },
    validateCompleteInput (e) {
      if (e.target.value.length > 0) {
        if (!email(e.target.value)) {
          this.invalid = true
          return false
        }
        this.invalid = false
      } else {
        this.invalid = false
        return false
      }
    },
    removeEmail (e) {
      if (e.target.tagName === 'I') {
        const tagLabel = e.target.getAttribute('data-item')
        const index = this.emails.indexOf(tagLabel)
        this.emails = [...this.emails.slice(0, index), ...this.emails.slice(index + 1)]
        this.addTags()
      }
    }
  }
}
</script>

<style scoped>
.tag-container {
  border: 2px solid #ccc;
  border-radius: 3px;
  background: #fff;
  display: flex;
  flex-wrap: wrap;
  align-content: flex-start;
  align-items: center;
  padding: 6px;
  overflow-x: scroll;
}
.tag-container .tag {
  height: 30px;
  margin: 5px;
  padding: 5px 6px;
  border: 1px solid #ccc;
  border-radius: 3px;
  background: #eee;
  display: flex;
  align-items: center;
  color: #333;
  box-shadow: 0 0 4px rgba(0, 0, 0, 0.2), inset 0 1px 1px #fff;
  cursor: default;
}
.tag i {
  font-size: 16px;
  color: rgba(0, 0, 0, 0.2);

  margin-left: 5px;
}
.tag-container input {
  padding: 5px;
  font-size: 16px;
  border: 0 !important;
  outline: none;
  font-family: 'Rubik';
  color: #333;
  flex: 1;
  margin: 0;
}

</style>
