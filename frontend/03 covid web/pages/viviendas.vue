<template>
  <v-container grid-list-xs>
    <template>
      <center>
        <h1>Gestión de viviendas</h1>
      </center>
      <v-form ref="form" v-model="valid" lazy-validation>
        <center>
          <h3>Formulario pendiente</h3>
        </center>
      </v-form>
    </template>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      valid: false,

      persona: {},
      reglas: [v => !!v || "El campo es obligatorio."],
      reglaCorreo: [
        v => !!v || "El campo es obligatorio.",
        v => /.+@.+\..+/.test(v) || "El correo no es correcto."
      ],
      tipo_documentos: ["CC", "CE"]
    };
  },
  methods: {
    guardarPersona() {
      if (this.$refs.form.validate()) {
        //Ejecutar el axios
        console.log(this.persona);
        let url = "http://localhost:3001/personas";
        this.$axios
          .post(url, this.persona)
          .then(respuesta => {
            console.log(respuesta);
          })
          .catch(error => {});
      }
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    }
  }
};
</script>
