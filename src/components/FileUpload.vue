<template>
  <div class="form-group">
    <input type="file" @change="onFileChange($event)" class="input-file" />
  </div>
  <div v-if="base64File">
    {{ base64File }}
  </div>
</template>

<script>
export default {
  name: 'FileUpload', 
  emits: ['fileEvent'],
  data() {
    return {
      base64File: ''
    }
  },
  methods: {
    onFileChange(event) {
      if (event.target.files && event.target.files.length > 0) {
        let file = event.target.files[0];
        let reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => {
          this.base64File = reader.result;
          this.$emit('fileEvent', this.base64File);
        }
      }
    }
  }
}
</script>

<style scoped>
.form-group {
  text-align: left;
}
</style>