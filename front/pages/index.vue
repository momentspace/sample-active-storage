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
      <UploadImage/>
      <spacers/>
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
      images: [
        {id: 1, description: "test", url: "https://pbs.twimg.com/profile_images/1237755166381592576/Vpfmfpxo_400x400.jpg"},
        {id: 2, description: "test2", url: "https://pbs.twimg.com/profile_images/875197603050176512/jrMv_xrc_400x400.jpg"},
        {id: 3, description: "test2", url: "https://pbs.twimg.com/profile_images/875197603050176512/jrMv_xrc_400x400.jpg"},
        {id: 4, description: "test2", url: "https://pbs.twimg.com/profile_images/875197603050176512/jrMv_xrc_400x400.jpg"},
        {id: 5, description: "test2", url: "https://pbs.twimg.com/profile_images/875197603050176512/jrMv_xrc_400x400.jpg"},
        {id: 6, description: "test2", url: "https://pbs.twimg.com/profile_images/875197603050176512/jrMv_xrc_400x400.jpg"},
        {id: 7, description: "test2", url: "https://pbs.twimg.com/profile_images/875197603050176512/jrMv_xrc_400x400.jpg"},
      ],
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
  },
  methods: {
    upload() {
      let result = this.$refs.form.validate();
      if (result) {
        let data = new FormData();
        data.append("image[description]", "sample");
        data.append('image[data]', this.file);
        this.send(data)
      }
    },
    rangeSize(size) {
      return size < 2000000;
    },
    isImage(type) {
      return ["image/jpeg", "image/png"].includes(type);
    },
    async send(data) {
      const config = {
        headers: { contentType: "multipart/form-data" }
      }
      const url = 'http://192.168.128.254:3100/api/v1/images/'
      const ret = await this.$axios.$post(url, data, config);
      console.log(ret);
    }
  }
}
</script>
