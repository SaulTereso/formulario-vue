<template>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">

<h1>Lista de Reservaciones</h1>
<div id="app">
    <div class="container">

        <input v-model='search' placeholder="Buscar reservacion..."/>
        <ul v-for="inf in searchData " :key="inf.name">
                <td>{{ inf.name }}</td>
                <td>{{ inf.app }}</td>
                <td>{{ inf.email }}</td>
        </ul>

    <table class="centered" id="expense">
        <thead>
            <tr>
                <th>ID</th>
                <th>Nombre</th>
                <th>Apellidos</th>
                <th>Correo</th>
                <th>Telefono</th>
                <th>Fecha de reservacion</th>
                <th>Hora de reservacion</th>
                <th>Servicio seleccionado por cliente</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="inf in info" :key="inf.name">
                <td>{{ inf.id }}</td>
                <td>{{ inf.name }}</td>
                <td>{{ inf.app }}</td>
                <td>{{ inf.email }}</td>
                <td>{{ inf.cel }}</td>
                <td>{{ inf.fecha }}</td>
                <td>{{ inf.horario }}</td>
                <td>{{ inf.selected }}</td>
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
      search: '',
    };
  },
  mounted() {
    axios
      .get("http://127.0.0.1:8000/api/citas")
      .then((response) => (console.log(response), (this.info = response.data)));
  },
  computed:{
    searchData: function () {
        var search = this.search;
        // console.log(search);
        
        if(search){
            return this.info.filter((resultados)=>
            resultados.name.toLowerCase().includes(search.toLowerCase())
            )
        }
        //return this.info;
    }
}
};
</script>