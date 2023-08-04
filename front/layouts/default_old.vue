<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="info" class="custom-navbar">
      <b-navbar-brand href="/">Teste Grupo Plan</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown right>
            <template #button-content>
              <em style="color: white"> Teste </em>
              <b-spinner small label="Spinning"></b-spinner>
            </template>
            <b-dropdown-item @click="">Logout</b-dropdown-item>
          </b-nav-item-dropdown>

          <b-nav-item-dropdown right>
            <template #button-content>
              <em style="color: white"
                >Clique aqui para fazer o login e poder cadastrar, editar, excluir
                os eletrodomésticos</em
              >
              <b-spinner small label="Spinning"></b-spinner>
            </template>

            <b-form @submit.prevent="" class="login-form">
              <b-form-group label="E-mail:">
                <b-form-input
                  v-model="login.email"
                  type="email"
                  required
                  placeholder="Digite o e-mail"
                ></b-form-input>
              </b-form-group>

              <b-form-group label="Senha:">
                <b-form-input
                  v-model="login.password"
                  type="password"
                  required
                  placeholder="Digite a senha"
                ></b-form-input>
              </b-form-group>

              <b-button type="button" @click="" variant="primary">
                Entrar
              </b-button>
            </b-form>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
    <b-container fluid class="mt-3">
      <Nuxt :key="componentKey"/>
    </b-container>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      icon: String,
      loading: false,
      snackbar: false,
      message: "",
      color: "",
      timeout: 6000,
      login: {
        email: "admin@admin.com",
        password: "password",
      },
      componentKey: 0
    };
  },

  methods: {
    /* async toast(title:string, message:string, variant:string) {
      this.$bvToast.toast(message, {
        title: title,
        variant: variant,
        solid: true,
        toaster: 'b-toaster-top-center',
        autoHideDelay: 5000,
      })
    }, */
  },
};
</script>

<style>
.custom-navbar {
  background-color: #800080 !important;
}
.login-form {
  max-width: 400px;
  width: 350px;
  padding: 20px;
}
</style>
