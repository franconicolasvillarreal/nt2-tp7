<template>

  <section class="src-components-formulario">
    <div class="text-left ml-3"><h4>Agregar tarea</h4></div>

    <form novalidate autocomplete="off" class="p-3 mt-3 text-left" @submit.prevent="enviar()">
      <div class="form-group  col-4">
          <label for="nombre">Nombre</label>
          <input 
            type="text"
            id="nombre"
            class="form-control"
            v-model="$v.f.nombre.$model"
          >
          <div v-if="$v.f.nombre.$error && $v.f.nombre.$dirty" class="alert alert-danger mt-1">
            <div v-if="$v.f.nombre.required.$invalid">Este campo es requerido</div>
          </div>
      </div>
          <div class="form-group col-4">
                <label for="email">Email</label>
                <input 
                    type="email"
                    id="email"
                    class="form-control"
                    v-model="$v.f.email.$model"
                >
                <div v-if="$v.f.email.$error && $v.f.email.$dirty" class="alert alert-danger mt-1">
                    <div v-if="$v.f.email.required.$invalid">Este campo es requerido</div>
                    <div v-if="$v.f.email.email.$invalid">Debe proveer un email válido</div>
                </div>
          </div>
          <div class="form-group  col-4">
                <label for="descripcion">Descripción</label>
                <input 
                    type="text-area"
                    id="descripcion"
                    class="form-control"
                    v-model="$v.f.descripcion.$model"
                >
                <div v-if="$v.f.descripcion.$error && $v.f.descripcion.$dirty" class="alert alert-danger mt-1">
                    <div v-if="$v.f.descripcion.required.$invalid">Este campo es requerido</div>
                    <div v-if="$v.f.descripcion.minLength.$invalid">Este campo debe tener al menos {{$v.f.descripcion.minLength.$params.min}} caracteres</div>
                    <div v-if="$v.f.descripcion.maxLength.$invalid">Este campo debe tener máximo {{$v.f.descripcion.maxLength.$params.max}} caracteres</div>
                </div>
          </div>
          <div class="form-group col-4">
                <input 
                    type="submit"
                    :disabled="false"
                    class="btn btn-info mt-4"
                    value="Agregar"
                >
          </div>
     
    </form>
  </section>

</template>

<script>
  import { required, email, minLength, maxLength } from '@vuelidate/validators'

  export default  {
    name: 'src-components-formulario',
    props: [],
    mounted () {

    },
    data () {
      return {
          f: this.resetForm(),
          url : 'https://5f93837c8742070016da699e.mockapi.io/nt2-tp6/tareas'
      }
    },
    validations: {
      f: {
        nombre: { 
          required
        },
        descripcion: { 
          required,
          minLength: minLength(10),
          maxLength: maxLength(50)
        },
        email: {
          required,
          email
        }
      }
    },
    methods: {
      async sendDatosFormAxios(datos) {
            try {
              if(datos){
                let res = await this.axios.post(this.url, datos, {'content-type': 'application/json'})
                console.log(res.data)
              }
            }
            catch(error) {
              console.log('HTTP POST ERROR', error)
            }
        },
        async getDatosFormAxios() {
            try {
              let res = await this.axios(this.url)
              console.log(res.data)
            }
            catch(error) {
              console.log('HTTP GET ERROR', error)
            }
        },
        async enviar() {
            this.$v.$touch()
            if(!this.$v.$invalid) {
              let form = this.f
              console.log(form)
              await this.sendDatosFormAxios(form)
              this.f = this.resetForm()
              this.$v.$reset()
            }
        },
        resetForm() {
            return {
               nombre: '',
               descripcion : '',
               email : ''
            }
        }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .src-components-formulario {

  }
</style>
