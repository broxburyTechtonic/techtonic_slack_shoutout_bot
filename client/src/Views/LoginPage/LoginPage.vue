<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent max-width="400px">
      <v-card class="login">
        <v-card-text class="login__form">
          <v-container>
            <v-row>
              <v-col cols="12">
                <v-text-field
                  @focus="handleFocus"
                  label="Email"
                  required
                  v-model="email"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  @focus="handleFocus"
                  label="Password"
                  type="password"
                  required
                  v-model="password"
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
          <v-alert
            class="login__alert"
            dense
            outlined
            type="error"
            v-if="error !== ''"
            >{{ error }}</v-alert
          >
          <div v-else class="login__error--placeholder"></div>
        </v-card-text>
        <v-card-actions class="login__actions">
          <v-spacer></v-spacer>
          <span class="login__disclaimer"
            >*Techtonic email account required</span
          >
          <v-btn color="blue darken-1" text @click="dialog = 'login'">
            Login
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
export default {
  name: 'LoginPage',
  data() {
    return {
      email: '',
      password: '',
    };
  },
  computed: {
    error() {
      return this.$store.state.loginError;
    },
    dialog: {
      get() {
        return true;
      },
      set() {
        const { email, password } = this;
        this.$store.dispatch('loginEmployee', { email, password });
      },
    },
  },
  methods: {
    handleFocus() {
      if (this.error !== '') this.$store.dispatch('resetError');
    },
    submitLogin() {
      const { email, password } = this;
      this.$store.dispatch('loginEmployee', { email, password });
    },
  },
};
</script>

<style lang="scss">
.login {
  &__form {
    padding-bottom: 0 !important;

    .v-alert {
      margin: -30px 10px 0 10px;
    }
  }
  &__actions {
    margin-top: 8px;
  }
  &__disclaimer {
    margin-right: 45px;
  }
  &__alert {
    margin-bottom: 0 !important;
  }
  &__error--placeholder {
    height: 32px;
  }
}
</style>
