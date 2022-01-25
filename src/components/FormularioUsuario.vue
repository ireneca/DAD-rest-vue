<template>
  <div id="formulario-persona">
    <form @submit.prevent="enviarFormulario">
      <div class="row">
        <div class="col-md-4">
          <label>Nombre</label>
          <input
            ref="name"
            v-model="usuario.name"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': procesando && nameInvalido }"
            @focus="resetEstado"
            @keypress="resetEstado"
          />
        </div>
        <div class="col-md-4">
          <label>Email</label>
          <input
            v-model="usuario.email"
            type="email"
            class="form-control"
            :class="{ 'is-invalid': procesando && emailInvalido }"
            @focus="resetEstado"
          />
        </div>
        <div class="col-md-4 m-auto d-grid">
          <button class="btn btn-primary">Añadir usuario</button>
        </div>
      </div>
      <div class="row mt-2">
        <div class="col-md-12">
          <div
            v-if="error && procesando"
            class="alert alert-danger"
            role="alert"
          >
            Debes rellenar todos los campos!
          </div>
          <div v-if="correcto" class="alert alert-success" role="alert">
            El usuario ha sido agregado correctamente!
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "formulario-usuario",
  data() {
    return {
      procesando: false,
      correcto: false,
      error: false,
      usuario: {
        nombre: "",
        email: "",
      },
    };
  },
  methods: {
    enviarFormulario() {
      this.procesando = true;
      this.resetEstado();

      // Comprobamos la presencia de errores
      if (this.nameInvalido || this.emailInvalido) {
        this.error = true;
        return;
      }

      this.$emit("crear-usuario", this.usuario);
      this.$refs.name.focus();

      this.error = false;
      this.correcto = true;
      this.procesando = false;

      // Restablecemos el valor de la variables
      this.usuario = {
        name: "",
        email: "",
      };
    },
    resetEstado() {
      this.correcto = false;
      this.error = false;
    },
  },
  computed: {
    nameInvalido() {
      return this.usuario.name.length < 1;
    },
    emailInvalido() {
      return this.usuario.email.length < 1;
    },
  },
};
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}
</style>