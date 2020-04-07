<template>
  <v-row class="pt-12" justify="center">
    <v-col cols="4" align="center">
      <v-card  class="mt-2 px-2">
        <v-card-title>Contact Me</v-card-title>
        <v-text-field label="Subject" v-model="subject"></v-text-field>
        <v-text-field label="Message" v-model="body"></v-text-field>
        <v-btn class="mb-2" @click="send(subject,body)">Send</v-btn>
      </v-card>
    </v-col>
  </v-row>

</template>

<script>
  export default {
    name: 'ContactPage',
    data: () =>({
      subject: null,
      body: null,

    }),
    methods:{
      send(subject,body){
        window.open(`mailto:meganwhitelavich@gmail.com?subject=${subject}&body=${body}`);
      }
    }
  }
</script>

<style scoped>

</style>
