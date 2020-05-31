<template>
  <v-row>
    <v-col>
      <v-card>
        <v-card-title class="headline">
          Active-Storage Sample Page
        </v-card-title>
        <v-form
          ref="form"
          v-model="valid"
          lazy-validation
        >
          <v-card-text>
            <v-file-input
              :rules="fileRules"
              accept="image/png, image/jpeg"
              label="image file"
              placeholder="placeholder"
              prepend-icon="mdi-camera"
              :clearable=true
              :show-size=true
              :counter=true
              required
              v-model="file"
            />
          </v-card-text>
          <v-card-actions>
            <v-spacer />
            <v-btn
              :disabled="!valid"
              color="primary"
              @click="upload"
            >
              upload
            </v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  components: {
  },
  data() {
    return {
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

