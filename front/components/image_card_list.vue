<template>
  <v-container>
    <v-row id="v-for-receipt">
      <v-col
        xs=12
        sm=12
        md=6
        lg=6
        xl=4
        v-for="image in images" :key="image.id">
        <v-card
          hover
        >
          <ImageCard 
            v-bind=image
          />
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import ImageCard from "@/components/image_card";
export default {
  props: ["images"],
  components: {
    ImageCard,
  },
  data() {
    return {
    }
  },
}
</script>
