<template>
  <v-row id="v-for-receipt">
    <v-col
      xs=12
      sm=12
      md=6
      lg=6
      xl=4
      v-for="receipt in receipts" :key="receipt.id">
      <ReceiptImage 
        v-bind=receipt
      />
    </v-col>
  </v-row>
</template>

<script>
import ReceiptImage from "@/components/receipt_image.vue";
export default {
  props: ["receipts"],
  components: {
    ReceiptImage
  },
  data() {
    return {
    }
  },
  mounted: function() {
    // indexを取得してimagesに設定する
//      const config = {
//        headers: { contentType: "multipart/form-data" }
//      }
//      const url = 'http://192.168.128.254:3100/api/v1/images/'
//      const ret = await this.$axios.$post(url, data, config);
//      console.log(ret);
  }
}
</script>
