<template>
  <v-app>
    <v-app-bar
            app
            color="primary"
            dark
            class="pb-8"
    >
      <v-app-bar-nav-icon @click="navDrawer = !navDrawer"/>
      <span>
        Bartender
      </span>
      <v-spacer/>
      <v-menu  offset-y  bottom :close-on-content-click="false">
        <template v-slot:activator="{ on }">
          <v-btn v-on="on" fab large icon ><v-icon>mdi-account</v-icon></v-btn>
        </template>
        <v-card width="500px">
          <v-card-title>Login</v-card-title>
          <v-row class="mx-2 pb-2 shrink" no-gutters >
            <v-col cols="12">
            <v-text-field label="Username"></v-text-field>
          </v-col>
            <v-col cols="12">
              <v-text-field label="Password" type="password"></v-text-field>
            </v-col>
            <v-slide-y-transition>
              <v-col cols="12" v-show="isRegister">
                <v-text-field label="Repeat Password" type="password"></v-text-field>
              </v-col>
            </v-slide-y-transition>

            <v-col >
              <v-btn>Login</v-btn>
            </v-col>
            <v-col >
              <v-btn @click="isRegister = !isRegister">Regester</v-btn>
            </v-col>
            <v-col  >
              <v-btn>Forgot Password</v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-menu>

    </v-app-bar>
    <v-navigation-drawer app v-model="navDrawer">
      <v-list>
        <v-list-item :input-value="homePage" @click="changePage('home')">
          <v-icon class="pr-2">mdi-home</v-icon>
          Home
        </v-list-item>
        <v-list-item @click="changePage('comingSoon')">
          <v-icon class="pr-2" >mdi-heart-outline</v-icon>
          Favorites
        </v-list-item>
        <v-list-item @click="changePage('comingSoon')">
          <v-icon class="pr-2" >mdi-plus</v-icon>
          New Drink
        </v-list-item>
        <v-list-item :input-value="contactPage" @click="changePage('contact')">
          <v-icon class="pr-2">mdi-card-account-mail-outline</v-icon>
          Contact
        </v-list-item>
        <v-list-item  :input-value="aboutPage" @click="changePage('about')">
          <v-icon class="pr-2">mdi-information-outline</v-icon>
          About
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <home-page v-if="homePage" class="mt-12"/>
    <contact-page v-if="contactPage"></contact-page>
    <about-page v-if="aboutPage"></about-page>
    <coming-soon-card v-if="comingSoon"/>
  </v-app>
</template>

<script>


  import HomePage from '@/components/Pages/HomePage';
  import ContactPage from '@/components/Pages/ContactPage';
  import AboutPage from '@/components/Pages/AboutPage';
  import ComingSoonCard from '@/components/ComingSoonCard';

  export default {
    name: 'App',

    components: {
      ComingSoonCard,
      HomePage,
      ContactPage,
      AboutPage,

    },

    data: () => ({
      navDrawer: false,
      homePage: true,
      contactPage: false,
      aboutPage: false,
      comingSoon: false,
      isRegister: false,

    }),
    methods: {
      changePage(to) {
        switch (to) {
          case 'home':
            this.homePage = true;
            this.contactPage = false;
            this.aboutPage = false;
            this.comingSoon = false;
            break;
          case 'contact':
            this.contactPage = true;
            this.homePage = false;
            this.aboutPage = false;
            this.comingSoon = false;
            break;
          case 'about':
            this.aboutPage = true;
            this.homePage = false;
            this.contactPage = false;
            this.comingSoon = false;
            break;
            case 'comingSoon':
              this.comingSoon = true;
              this.aboutPage = false;
              this.contactPage = false;
              this.homePage = false;

        }
      }
    },
  };
</script>
