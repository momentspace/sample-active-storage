<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-container
      md-10
      sm-10
      xs-12
    >
      <UploadImage
        :uploadAction=upload
      />
      <v-btn @click=refresh>sync</v-btn>
      <ReceiptList
        v-bind:receipts=images
      />
    </v-container>
  </v-layout>
</template>

<script>
import UploadImage from "@/components/upload_image.vue";
import ReceiptList from "@/components/receipt_list.vue";
export default {
  components: {
    UploadImage,
    ReceiptList
  },
  data() {
    return {
      images: [],
      valid: false,
      file: null,
      fileRules: [
        value => {
          if (value && this.isImage(value.type) && this.rangeSize(value.size)) {
            return true;
          }
          return false;
        }
      ],
    }
  },
  mounted: function() {
    this.valid = false;
    this.refresh();
  },
  methods: {
    upload(description, file) {
      let data = new FormData();
      data.append("image[description]", description);
      data.append('image[data]', file);
      this.uploadImage(data)
      this.refresh();
    },
    rangeSize(size) {
      return size < 2000000;
    },
    isImage(type) {
      return ["image/jpeg", "image/png"].includes(type);
    },
    async uploadImage(data) {
      const config = {
        headers: { contentType: "multipart/form-data" }
      }
      const url = 'http://192.168.128.254:3100/api/v1/images/'
      const ret = await this.$axios.$post(url, data, config);
      console.log(ret);
    },
    async refresh() {
      const url = 'http://192.168.128.254:3100/api/v1/images/'
      const ret = await this.$axios.$get(url);
      console.log(ret);
      this.images = ret;
    },
  }
}
</script>
