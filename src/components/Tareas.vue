<template>

  <section class="src-components-tareas">
    <div class="text-left ml-3"><h4>Lista de tareas</h4></div>

    <div v-if="tareas.length">

          <table class="table text-left">
            <tr>
                <th scope="col">ID</th>
                <th scope="col">Fecha</th>
                <th scope="col">Nombre</th>
                <th scope="col">Email</th>
                <th scope="col">Descripcion</th>
            </tr>
            <tr class=""
            v-for="(tarea, index) in tareas"
            :key="index">
                <td scope="col"><p>{{ tarea.id }}</p></td>
                <td scope="col"><p>{{ formatearFecha(tarea.createdAt) }}</p></td>
                <td scope="col"><p>{{ tarea.nombre }}</p></td>
                <td scope="col"><p>{{ tarea.email }}</p></td>
                <td scope="col"><p>{{ tarea.descripcion }}</p></td>   
                <td scope="col"><button class="btn btn-danger" @click="deleteTareaAxios(tarea.id)">DEL</button></td>                       
            </tr>
          </table>
      
    </div>
    <h2 v-else class="alert alert-warning">No se encontraron tareas.</h2>
  </section>

</template>

<script>
  import filters from '../filters.js'
  export default  {
    name: 'src-components-tareas',
    props: [],
    mixins: [
      filters
    ],
    mounted () {
      this.getDatosFormAxios()
    },
    data () {
      return {
        url : 'https://5f93837c8742070016da699e.mockapi.io/nt2-tp6/tareas/',
        tareas : []
      }
    },
    methods: {
      async getDatosFormAxios() {
            try {
              let res = await this.axios(this.url)
              console.log(res.data)
              this.tareas = res.data
            }
            catch(error) {
              console.log('HTTP GET ERROR', error)
            }
        },
        deleteTareaAxios(id) {
          console.log('delete',id)

          this.axios.delete(this.url+id)
            .then(res => {
              let tarea = res.data
              console.log(tarea)
              let offset = this.tareas.findIndex(tarea => tarea.id == id)
              this.tareas.splice(offset,1)
            })
            .catch(error => console.log('HTTP DELETE ERROR', error))
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .src-components-tareas {

  }
</style>
