<template>
  <v-container grid-list-xs>
    <template>
      <center>
        <h1>Gestión de personas</h1>
      </center>
      <v-form ref="form" v-model="valid" lazy-validation>
        <v-select
          v-model="persona.tipo_documento"
          :items="tipo_documentos"
          :rules="[v => !!v || 'El tipo de documento es obligatorio']"
          label="Tipo de documento"
          required
        ></v-select>

        <v-text-field
          v-model="persona.documento"
          :counter="15"
          :rules="reglas"
          label="Documento"
          required
        ></v-text-field>

        <v-text-field v-model="persona.nombre" :rules="reglas" label="Nombre" required></v-text-field>

        <v-text-field
          v-model="persona.celular"
          :counter="10"
          :rules="reglas"
          label="Celular"
          required
        ></v-text-field>

        <v-text-field v-model="persona.correo" :rules="reglaCorreo" label="Correo" required></v-text-field>

        <v-btn :disabled="!valid" color="success" class="mr-4" @click="guardarPersona">Guardar</v-btn>
      </v-form>
    </template>

    <v-data-table :headers="encabezados" :items="personas" :items-per-page="5" class="elevation-1"></v-data-table>
    <v-snackbar v-model="toast">
      {{ mensaje }}
      <v-btn color="pink" text @click="toast = false">Close</v-btn>
    </v-snackbar>
  </v-container>
</template>

<script>
import config from "@/assets/config/index";
export default {
  beforeMount() {
    this.cargarPersonas();
  },
  data() {
    return {
      valid: false,
      toast: false,
      persona: {},
      reglas: [v => !!v || "El campo es obligatorio."],
      reglaCorreo: [
        v => !!v || "El campo es obligatorio.",
        v => /.+@.+\..+/.test(v) || "El correo no es correcto."
      ],
      tipo_documentos: ["CC", "CE"],
      encabezados: [
        { text: "Tipo de documento", value: "tipo_documento" },
        { text: "Documento", value: "documento" },
        { text: "Nombre", value: "nombre" },
        { text: "Correo", value: "correo" },
        { text: "Celular", value: "celular" }
      ],
      personas: [],
      mensaje: null
    };
  },
  methods: {
    guardarPersona() {
      if (this.$refs.form.validate()) {
        //Ejecutar el axios
        console.log(this.persona);
        let url = config.url_api;
        this.$axios
          .post(url, this.persona)
          .then(respuesta => {
            console.log(respuesta);
          })
          .catch(error => {});
      }
    },
    cargarPersonas() {
      let url = config.url_api;
      this.$axios
        .get(url + "personas")
        .then(respuesta => {
          let data = respuesta.data;
          if (data.ok) {
            this.personas = data.info;
          }
          this.toast = true;
          this.mensaje = data.mensaje;
        })
        .catch(error => {
          this.toast = true;
          this.mensaje = "Ha ocurrido un error, intenta de nuevo. ";
        });
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
