<template>
  <v-app>
    <HelloWorld/>
    <v-content>
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>
import HelloWorld from '@/components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
<style scoped>
  @font-face {
    font-family: Vazir;
    src: url('assets/fonts/vazir/Vazir-Medium.woff2');
    font-weight: normal;
  }
  @font-face {
    font-family: IRANSans;
    src: url('assets/fonts/Farsi_numerals/IRANSans(FaNum)_Medium.ttf');
    font-weight: normal;
  }
  .v-application {
    font-family: Vazir;
  }


</style>
