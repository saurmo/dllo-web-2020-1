<template>
  <v-app>
    <!-- Menu lateral -->
    <v-navigation-drawer v-model="drawer" clipped app>
      <v-list>
        <v-list-item
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title v-text="item.title" />
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <!-- Barra superior -->
    <v-app-bar clipped-left app>
      <!-- Icono menú -->
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn text class="text-none" color="secondary"
        >Hola {{ usuario.nombre }}!</v-btn
      >
      <v-spacer />

      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn text class="text-none" color="secondary">
        <v-icon>mdi-account-edit</v-icon>Editar mi perfil
      </v-btn>
      <v-btn text class="text-none" color="error">
        <v-icon>mdi-logout</v-icon>Salir
      </v-btn>
    </v-app-bar>
    <v-content>
      <v-container>
        <nuxt />
      </v-container>
    </v-content>

    <v-footer fixed app>
      <v-spacer></v-spacer>
      <span>&copy; {{ new Date().getFullYear() }}</span>
      <small>saurmo</small>
      <v-spacer></v-spacer>
    </v-footer>
  </v-app>
</template>

<script>
import config from "@/assets/config/index";

export default {
  beforeMount() {
    this.loadPage();
  },
  data() {
    return {
      drawer: false,
      items: [
        {
          icon: "mdi-account-multiple",
          title: "Personas",
          to: "/personas"
        },
        {
          icon: "mdi-home-group",
          title: "Viviendas",
          to: "/viviendas"
        }
      ],
      usuario: {},
      title: "COVID 19"
    };
  },
  methods: {
    loadPage() {
      let url = config.url_api + "verificar";
      let token = localStorage.getItem("token");
      this.$axios.setHeader("token", token);

      this.$axios
        .get(url, { headers: { token } })
        .then(response => {
          this.usuario = response.data.info;
        })
        .catch(error => {
          console.log(error);
        });
    }
  }
};
</script>
