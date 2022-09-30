<template>
  <div class="about">
    <v-card></v-card>
    <v-card>
      <v-carousel touch>
        <v-card-title>errors in production</v-card-title>
        <b-aspect aspect="16:9">
        <v-carousel-item
        v-for="error in errors"
        :key="error.src"
        :src="error.src"
        reverse-transition="fade-transition"
        transition="fade-transition"
        >
        <v-card>
          <!-- <v-subheader>{{error.alt}}</v-subheader> -->
        </v-card>
      </v-carousel-item>
    </b-aspect>
      </v-carousel>
    </v-card>
  </div>
</template>
<script>
export default {
  name: "Home",
  data: () => ({
    errors: [
      { src: "https://raw.githubusercontent.com/Abdool-Zaid/vuetodo/main/src/assets/errors/Capture.PNG", alt: "violation" },
      { src: "https://raw.githubusercontent.com/Abdool-Zaid/vuetodo/main/src/assets/errors/broke%20matrix.PNG", alt: "broke the matrix" },
      { src: "https://raw.githubusercontent.com/Abdool-Zaid/vuetodo/main/src/assets/errors/max%20stack.PNG", alt: "no longer fullsatck I am now overstacked" },
      { src: "https://raw.githubusercontent.com/Abdool-Zaid/vuetodo/main/src/assets/errors/no%20id.PNG", alt: "no id ..." },
    ],
  }),
};
</script>
