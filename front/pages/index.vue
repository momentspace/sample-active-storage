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
      <ImageUploader
        :uploadAction="upload"
        maxSize=5000000
      />
      <ImageCardList
        v-bind:images=images
      />
    </v-container>
  </v-layout>
</template>

<script>
import ImageUploader from "@/components/image_uploader.vue";
import ImageCardList from "@/components/image_card_list.vue";

const baseurl = `http://${location.hostname}`;
const baseurlApi = `http://${location.hostname}:3100/`;
export default {
  components: {
    ImageUploader,
    ImageCardList 
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
    this.initialize();
  },
  methods: {
    upload(description, file) {
      let data = new FormData();
      data.append("image[description]", description);
      data.append('image[data]', file);
      this.uploadImage(data)
    },
    async uploadImage(data) {
      const config = {
        headers: { contentType: "multipart/form-data" }
      }
      const url = `${baseurlApi}/api/v1/images/`
      const ret = await this.$axios.$post(url, data, config);
      this.images.unshift(ret)
      console.log(ret);
    },
    async initialize() {
      const url = `${baseurlApi}/api/v1/images/`
      const ret = await this.$axios.$get(url);
      console.log(ret);
      this.images = ret;
    },
  }
}
</script>
