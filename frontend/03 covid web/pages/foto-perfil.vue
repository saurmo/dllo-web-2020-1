<template>
  <div>
    <input type="file" ref="archivo" name="archivo" />
    <v-btn color="success" @click="actualizarFoto">Actualizar foto</v-btn>
  </div>
</template>
<script>
import config from "@/assets/config/index";

export default {
  methods: {
    actualizarFoto() {
      let url = config.url_api + "personas/imagenes";
      let data = this.$refs.archivo.files[0];
      console.log(data);
      let formData = new FormData();
      formData.append("archivo", data);
      this.$axios
        .post(url, formData)
        .then(respuesta => {
          console.log(respuesta);
        })
        .catch(error => {});
    }
  }
};
</script>