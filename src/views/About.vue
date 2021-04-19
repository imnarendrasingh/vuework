<template>
  <div class="about">
    <div class="container">
      <div class="form-group">
        <div v-for="(item, index) in files" :key="index" class="form-row">
          <div class="col-8">
            <FileUpload @file-event="onFileChanged($event, index)" />
          </div>
          <div class="col-4 text-left">
            <button v-if="index == files.length - 1 && files.length < 5" @click="addFileRow()" type="button" class="icon-button btn btn-info btn-sm mr-2">
              <i class="fa fa-plus" aria-hidden="true"></i>
            </button>
            <button v-if="files.length > 1" @click="removeFileRow(index)" type="button" class="icon-button btn btn-danger btn-sm">
              <i class="fa fa-minus" aria-hidden="true"></i>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import FileUpload from "@/components/FileUpload.vue";

export default {
  name: "About",
  components: {
    FileUpload,
  },
  data() {
    return {
      files: [],
    };
  },
  created() {
    this.files.push({
      value: ''
    })
  },
  methods: {
    addFileRow() {
      let last = this.files.length - 1;
      // if (this.files[last]['value'] == '') {
      //   alert('Please select file first.');
      // } else if (this.files.length < 5) {
      //   this.files.push({
      //     value: ''
      //   });
      // }
      if (this.files.length < 5) {
        this.files.push({
          value: ''
        });
      }
    },
    removeFileRow(index) {
      this.files.splice(index, 1);
    },
    onFileChanged(event, index) {
      this.files[index]['value'] = event;
    }
  }
};
</script>