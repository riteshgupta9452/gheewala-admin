<template>
    <CFormLabel for="select_icon">{{ label }}</CFormLabel>
    <CFormInput id="select_icon" type="file" :placeholder="label" accept="image/*" @change="uploadIconImage" />
    <span v-if="isUploading">Uploading...</span>
</template>

<script>
import upload from "@/mixins/upload.vue";
export default {
    name: 'FileUploaderComponent',
    props: ["label"],
    mixins: [upload],
    data() { return { uploadString: "", isUploading: false } },
    methods: {
        async uploadIconImage($event) {
            this.isUploading = true
            const input = $event.target;
            let file = input.files && input.files[0] ? input.files[0] : null
            if (!file) {
                return;
            }
            this.uploadString = await this.uploadFile(file);
            this.isUploading = false
            this.$emit("upload", this.uploadString)
        }
    }
}
</script>
  