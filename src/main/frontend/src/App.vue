<template>
  <div id="app">
    <h1>
      <img src="./assets/logo.svg" alt="Enroller" class="logo">
      System do zapisów na zajęcia
    </h1>
    <div v-if="authenticatedUsername">
      <h2>Witaj {{ authenticatedUsername }}!
        <a @click="logout()" class="float-right  button-outline button">Wyloguj</a>
      </h2>
      <meetings-page :username="authenticatedUsername"></meetings-page>
    </div>
    <div v-else>
      <button @click="registering= false" :class="!registering ? '' : 'button-clear'">Zaloguj się</button>
      <button @click="registering= true" :class="registering ? '' : 'button-clear'">Zarejestruj się</button>
      <div :class="isErrorMessage ? 'red' : 'green'" v-if="message"> {{message}} </div>
      <login-form @login="login($event)" v-if="registering === false"></login-form>
      <login-form @login="register($event)" button-label="Zarejestruj się" v-else></login-form>
    </div>
  </div>
</template>

<script>
    import "milligram";
    import LoginForm from "./LoginForm";
    import MeetingsPage from "./meetings/MeetingsPage";

    export default {
        components: {LoginForm, MeetingsPage},
        data() {
            return {
                registering: false,
                authenticatedUsername: "",
                message: '',
                isErrorMessage: false,

            };
        },
        methods: {
            login(user) {
                this.$http.post('tokens', user)
                //this.authenticatedUsername = user.login;


            },
            logout() {
                this.authenticatedUsername = '';
            },
            register(user) {
                this.$http.post('participants', user) //nawiazywanie polaczenia z backendem (dodawanie uzytkownika) - zlecamy żądanie
                .then(response => { //jesli wroci odpowiedź na żądanie to rób to co poniżej
                    this.message = "Udało się!";
                    this.isErrorMessage = false;
                })
                .catch(response => { //jesli NIE wróci odpowiedź na żądanie to rób to co poniżej
                    this.message = "Nie udało się!";
                    this.isErrorMessage = true;
                });
            }
        }
    };
</script>

<style>
  #app {
    max-width: 1000px;
    margin: 0 auto;
  }

  .logo {
    vertical-align: middle;
  }

  .red { background: red; }

  .green { background: #8eea8e; }


</style>

