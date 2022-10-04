<template>
<!-- <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css"> -->
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.9.1/font/bootstrap-icons.css">

<nav class="navbar bg-light">
  <div class="container-fluid">
    <a class="navbar-brand">
      <img src="https://salonyspasv.com/wp-content/uploads/2022/09/new_logo.jpeg" alt="Logo" width="100" height="100"  class="d-inline-block align-text-center">
      Salon & Spa Saldivar Villar - Consulta todas las reservaciones de los clientes
    </a>
  </div>
</nav>

<h1>Reservaciones</h1>
<div id="app">
    <div class="container">

        <input class="form-control me-2" v-model='search' placeholder="Buscar reservacion..."/>
        <ul v-for="inf in searchData " :key="inf.id">
                <td>{{ inf.name }} </td>
                <td>{{ inf.app }} </td>
                <td>{{ inf.email }}</td>
        </ul>

    <table class="table table-striped">
        <thead>
            <tr>
                <th>Nombre</th>
                <th>Apellidos</th>
                <th>Correo</th>
                <th>Telefono</th>
                <th>Fecha de reservacion</th>
                <th>Hora de reservacion</th>
                <th>Servicio seleccionado por cliente</th>
                <th>Status del servicio</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="inf in info" :key="inf.name">
                <td>{{ inf.name }}</td>
                <td>{{ inf.app }}</td>
                <td>{{ inf.email }}</td>
                <td>{{ inf.cel }}</td>
                <td>{{ inf.fecha }}</td>
                <td>{{ inf.horario }}</td>
                <td>{{ inf.selected }}</td>
                <td class="td-actions text-right">
                    <button class="btn btn-outline-success btn-sm" type="button">
                        <i class="bi bi-check2-all">Completo</i>
                    </button>
                    <button class="btn btn-outline-danger btn-sm" type="button">
                        <i class="bi bi-x-lg">Incompleto</i>
                    </button>
                </td>
            </tr>
        </tbody>
    </table>
    </div>
</div>
</template>

<script type="text/javascript">
import axios from "axios";
export default {
  el: "#app",
  data() {
    return {
      info: null,
      errored: false,
      loading: true,
      search: '',
    };
  },
  mounted() {
    axios
      .get("http://127.0.0.1:8000/api/citas")
      .then((response) => (console.log(response), (this.info = response.data)))
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  },
  computed:{
    searchData: function () {
        var search = this.search;
        // console.log(search);
        
        if(search){
            return this.info.filter((resultados)=>{
            if(resultados.name.toLowerCase().includes(search.toLowerCase())||resultados.email.toLowerCase().includes(search.toLowerCase())||resultados.app.toLowerCase().includes(search.toLowerCase())){
                return resultados;
            }
            }
            
            )
        }
        //return this.info;
    }
}
};
</script>