<template>
  <v-card class="px-4">
    <v-card-text>
      <v-form ref="loginForm" v-model="valid" lazy-validation>
        <v-row>
          <v-col cols="12">
            <v-text-field
              v-model="email"
              :rules="emailRules"
              label="E-mail"
              required
            ></v-text-field>
          </v-col> 
          <v-col cols="12">
            <v-text-field
              v-model="password"
              :rules="[rules.required, rules.min]"
              :type="show ? 'text' : 'password'"
              label="Пароль"
              hint="Как минимум 6 символов"
              counter
              @click:append="show = !show"
            ></v-text-field>
          </v-col>
          <v-col class="d-flex" cols="12" sm="6" xsm="12"> </v-col>
          <v-spacer></v-spacer>
          <v-col class="d-flex" cols="12" sm="3" xsm="12" align-end>
            <v-btn
              x-large
              block
              text
              outlined
              :disabled="!valid"
              color="#4682b4"
              @click.prevent="login"
            >
            Логин
            </v-btn>
          </v-col>
        </v-row>
      </v-form>
    </v-card-text>
  </v-card>
</template>

<script>
import firebase from "firebase/compat/app";
import "firebase/compat/auth";

export default {
  data() {
    return {
      valid: false,
      show: false,
      password: "",
      email: "",
      emailRules: [
        (v) => !!v || "Required",
        (v) => /.+@.+\..+/.test(v) || "E-mail должен быть реальным",
      ],
      rules: {
        required: (value) => !!value || "Required.",
        min: (v) => (v && v.length >= 6) || "Минимум 6 символов",
      },
    };
  },
  methods: {
    async login() {
      const isValid = this.$refs.loginForm.validate();

      if (isValid) {
        try {
          this.$store.commit("setLoading", true);
          this.formValidMessage = "";
          const firebaseAuth = await firebase.auth();
          await firebaseAuth.signInWithEmailAndPassword(
            this.email,
            this.password
          );
          this.$router.push("/");
        } catch (e) {
          console.log(e);
          this.$store.commit("setMessage", e.code);
        } finally {
          this.$store.commit("setLoading", false);
        }
      }
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
  },
};
</script>

<style></style>
