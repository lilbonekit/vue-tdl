<template>
  <div class="add-pannel">
    <h2>Add your todo here:</h2>
    <form @submit.prevent="handleSubmit">
      <label>
        <input 
          type="text"
          required
          v-model="inputValue"
          placeholder="Put the task name..."
        />
      </label>
      <label>
        <textarea
          required
          v-model="textareaValue" 
          placeholder="Put the description..."
        />
      </label>
      <input 
        type="submit"
        value="Add the todo"
      >
    </form>
  </div>
</template>

<script>
export default {
  name: 'add-panel',
  data() {
    return {
      inputValue: '',
      textareaValue: ''
    }
  },
  methods: {
    isEmpty(value) {
      return value.trim() === '';
    },
    clearInputs() {
      this.inputValue = ''
      this.textareaValue = ''
    },
    handleSubmit() {
      // Чтобы не отправить в описание textarea состоящий только из пробелов
      if(this.isEmpty(this.inputValue) || this.isEmpty(this.textareaValue)) return alert('Paste data!')
      this.$emit('inputs-to-parent', {
        title: this.inputValue.trim(),
        descr: this.textareaValue
      })
      this.clearInputs()
    }
  }
}
</script>

<style lang="scss" scoped>
.add-pannel {
  display: flex;
  flex-direction: column;
  gap: 10px;
  flex-grow: 1;
  padding: 10px;
  background-color: #2d2d2d;
  border-radius: 20px;
  height: fit-content;
}

form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

h2 {
  color: white;
}

label,
input,
textarea {
  display: flex;
  border-radius: 10px;
  resize: none;
  border: none;
  width: 100%;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

input,
textarea {
  padding: 15px;
}

textarea {
  height: 100px;
  align-items: flex-start;
}

input[type='submit'] {
  display: flex;
  justify-content: center;
  font-weight: 700;
  cursor: pointer;
}

</style>