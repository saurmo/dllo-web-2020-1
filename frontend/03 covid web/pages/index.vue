<template>
  <v-row align="center" justify="center">
    <v-col cols="12" sm="8" md="4">
      <!--Card login -->
      <v-card class="elevation-12">
        <v-card-title primary-title>
          <v-spacer />
          <span>Iniciar sesión</span>
          <v-spacer />
        </v-card-title>

        <v-card-text>
          <v-form ref="formularioLogin">
            <v-text-field
              label="Identificación"
              v-model="usuario.documento"
              prepend-icon="mdi-account"
              type="text"
              :rules="reglas"
              required
            />

            <v-text-field
              v-model="usuario.clave"
              label="Contraseña"
              name="password"
              required
              :rules="reglas"
              prepend-icon="mdi-lock"
              type="password"
            />
          </v-form>
        </v-card-text>
        <v-card-actions v-if="mensaje">
          <v-spacer />
          <span class="red--text py-2">{{ mensaje }}</span>
          <v-spacer />
        </v-card-actions>
        <v-divider></v-divider>
        <v-card-actions>
          <v-spacer />
          <v-btn color="primary" class="text-none" @click="iniciar">Ingresar</v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import config from "@/assets/config/index";
export default {
  //Asigno el layout
  layout: "blank",
  data() {
    return {
      usuario: {},
      mensaje: null,
      reglas: [v => !!v || "El campo es obligatorio."]
    };
  },
  methods: {
    iniciar() {
      this.mensaje = null;

      if (this.$refs.formularioLogin.validate()) {
        let url = config.url_api + "login";

        this.$axios
          .post(url, this.usuario)
          .then(response => {
            let data = response.data;
            localStorage.setItem("token", data.info);
            this.$cookies.set("userid", this.usuario.documento);
            let userid = this.$cookies.get("userid");
            console.log("USERID COOKIE", userid);
            //Enviar al usuario a la página del home
            this.$axios.setHeader("token", data.info);
            this.$router.push("/home");
          })
          .catch(error => {
            console.log(error.response);
            if (error.response && error.response.data) {
              this.mensaje = "Error:" + error.response.data.mensaje;
            } else {
              this.mensaje = "Error:" + error;
            }
          });
      } else {
        this.mensaje = "El formulario está incompleto.";
      }
    }
  }
};
</script>
