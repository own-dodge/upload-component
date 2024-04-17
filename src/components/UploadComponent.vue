<template>
  <div class="root">
    <label class="input-file">
      <div class="label-text">{{ labelValue }}</div>
      <input v-if="!isFileUpload" type="file" name="file" @change="handleFileUpload">
      <input v-if="isFileUpload" type="button" name="file" @click="handleFileUpload">
      <span class="input-file-btn">{{ inputValue }}</span>
      <div class="file-text-block">
        <div v-if="isLoading" class="loader"></div>
        <span class="input-file-text">{{ fileValue }}</span>
      </div>
    </label>

    <div class="warning-text-block">
      <span v-if="!error">{{ hintText }}</span>
      <span v-if="error" class="error-text">Error message</span>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    labelValue: {
      type: String,
      default () {
        return 'Label'
      }
    },
    hintText: {
      type: String,
      default () {
        return 'Hint text'
      }
    },
    error: {
      type: Boolean,
      default () {
        return false
      }
    },
    errorText: {
      type: String,
      default () {
        return 'Error message'
      }
    }
  },
  data () {
    return {
      inputValue: 'Выбрать файл' as string,
      fileValue: 'Файл не выбран' as string,
      file: null as File | null,
      timeout: 0 as number
    }
  },
  computed: {
    isFileUpload (): boolean {
      return this.file !== null
    },
    isLoading (): boolean {
      return this.inputValue === 'Отменить'
    }
  },
  methods: {
    handleFileUpload (event: Event) {
      if (this.isFileUpload) {
        clearTimeout(this.timeout)
        this.file = null
        this.fileValue = 'Файл не выбран'
        this.inputValue = 'Выбрать файл'
        this.$emit('update:modelValue', this.file)
        return
      }

      const files = (event.target as HTMLInputElement).files
      if (files) {
        this.file = files[0]
        this.fileValue = this.file.name

        this.inputValue = 'Отменить'
        this.timeout = setTimeout(() => {
          this.inputValue = 'Удалить'
        }, 3000)
        this.$emit('update:modelValue', this.file)
      }
    }
  }
})
</script>

<style scoped>
.root {
  width: 100%;
  padding: 20px;
}

.label-text {
  margin-bottom: 12px;
  font-size: 13px;
  line-height: 15.6px;
}

.input-file {
  position: relative;
  display: inline-block;
  margin-bottom: 8px;
}

.input-file-btn {
  position: relative;
  display: inline-block;
  cursor: pointer;
  outline: none;
  text-decoration: none;
  font-size: 14px;
  vertical-align: middle;
  color: #384252;
  text-align: center;
  border-radius: 12px;
  background-color: #fff;
  padding: 12px;
  box-sizing: border-box;
  border: 1px solid;
  margin: 0;
  transition: background-color 0.2s;
}

.input-file-text {
  display: inline-block;
  color: #9CA3B0;
  font-size: 14px;
  line-height: 16px;
}

.input-file input {
  position: absolute;
  z-index: -1;
  opacity: 0;
  display: block;
  width: 0;
  height: 0;
}

.input-file input[type=file]:focus + .input-file-btn {
  box-shadow: 0px 0px 0px 4px #6B72801A;
}

.input-file:hover .input-file-btn {
  background-color: #F3F4F6;
}
.input-file:active .input-file-btn {
  background-color: #E5E7EB;
}

.input-file input[type=file]:disabled + .input-file-btn {
  color: #9CA3B0;
}

.warning-text-block {
  font-size: 13px;
  line-height: 15.6px;
  color: #6B7280;
}

.error-text {
  color: #EF4343;
}

.file-text-block {
  display: inline;
  position: relative;
  padding-left: 16px;
}

.loader {
  display: inline-block;
  position: relative;
  margin-right: 8px;
  top: 5px;
  width: 16px;
  height: 16px;
  border: 2px solid #fff;
  border-radius: 50%;
  border-top: 2px solid #00A8A5;
  -webkit-animation: spin 2s linear infinite;
  animation: spin 2s linear infinite;
}

@-webkit-keyframes spin {
  0% { -webkit-transform: rotate(0deg); }
  100% { -webkit-transform: rotate(360deg); }
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
