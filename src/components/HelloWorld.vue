<template> 
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
  <div class="container-md">
    <br>
    <h1>Reserva tu cita</h1>
      <img alt="Vue logo" src="../assets/Portada.png" width="198">
        <form id="myform" @submit="validateForm" method="post" action="{{route('save')}}">

          <div class="row g-3">
            <div class="col-md-4">
              <label for="name">Nombre</label>
              <input v-model="name" type="text" placeholder="Juan" class="form-control" aria-label="name">
            </div>
            <div class="col-md-4">
              <label for="app">Apellidos</label>
              <input v-model="app" type="text" placeholder="Martinez Martinez" class="form-control" aria-label="app">
              </div>
            <div class="col-md-4">
              <label for="email">Correo</label>
              <input v-model="email" type="email" class="form-control" placeholder="example@gmail.com" aria-label="email">
            </div>
          </div>
          <br>

          <div class="row g-3">
            <div class="col-md-6">
              <label for="cel">Telefono</label>
              <input v-model="cel" type="text"  class="form-control" placeholder="+52 7221012210" aria-label="cel">
            </div>

            <div class="col">
              <label for="date">Fecha que desea reservar</label>
              <input v-model="fecha" type="date"  :min="minDate" :max="maxDate" class="form-control">
            </div>
          </div>
          <br>
          
          <svg xmlns="http://www.w3.org/2000/svg" style="display: none;">
          <symbol id="check-circle-fill" viewBox="0 0 16 16">
            <path d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zm-3.97-3.03a.75.75 0 0 0-1.08.022L7.477 9.417 5.384 7.323a.75.75 0 0 0-1.06 1.06L6.97 11.03a.75.75 0 0 0 1.079-.02l3.992-4.99a.75.75 0 0 0-.01-1.05z"/>
          </symbol>
          
          <symbol id="exclamation-triangle-fill" viewBox="0 0 16 16">
            <path d="M8.982 1.566a1.13 1.13 0 0 0-1.96 0L.165 13.233c-.457.778.091 1.767.98 1.767h13.713c.889 0 1.438-.99.98-1.767L8.982 1.566zM8 5c.535 0 .954.462.9.995l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 5.995A.905.905 0 0 1 8 5zm.002 6a1 1 0 1 1 0 2 1 1 0 0 1 0-2z"/>
          </symbol>
          </svg>

          <!--Select con API-->
          <div class="col">
            <label for="service">Nuestros Servicios: </label>
            <section v-if="errored" class="alert alert-danger d-flex align-items-center">
              <svg class="bi flex-shrink-0 me-2"><use xlink:href="#exclamation-triangle-fill"/></svg>
              <strong>Lo sentimos, no es posible obtener la información en este momento, por favor recargue nuevamente o intente más tarde</strong>
            </section>

            <section v-else>
              <div v-if="loading" class="alert alert-success d-flex align-items-center">
                <svg class="bi flex-shrink-0 me-2"><use xlink:href="#check-circle-fill"/></svg><strong>Cargando Información...</strong></div>
              <div v-else v-for="inf in info" :key='inf.id'></div>
            </section>

            <select v-model="selected" class="form-select">
              <option disabled value="">Por favor, seleccione uno</option>
              <option id="selected" v-for="inf in info" v-bind:value="inf.nombre_servicio" :key='inf.nombre_servicio' >{{inf.nombre_servicio}}</option>
            </select>
          </div>
          <br>
          <div class="col">
            <span>Servicio Seleccionado: {{ selected }}</span>
          </div>
          <!--Fin del select con API-->
          
          <!--Prueba de validacion del formulario-->
          <p>
            <input type="submit" class="btn btn-outline-success col-sm-2 m-3" value="Reservar">
          </p>
          <!--Fin prueba de validacion del formulario-->

        <!--div>
          <a href="#" type="button" class="btn btn-outline-success col-sm-2 m-3">Reservar</a>
          <a href="#" type="button" class="btn btn-outline-danger col-sm-2">Cancelar</a> 
        </div-->
      </form>
      </div>


</template>

<script setup>
import {computed} from 'vue'
    const minDate = computed(()=>{
          const fecha = new Date()
          let day = fecha.getDate();
          let month = fecha.getMonth();
          month += 1; 
          let year = fecha.getFullYear();

          day   = ('0' + day).slice(-2);
          month = ('0' + month).slice(-2);

          return `${year}-${month}-${day}`
    })

    const maxDate = computed(() => {
      const f = new Date()
      let day = f.getDate()
      let month = f.getMonth();
          month += 2; 
          let year = f.getFullYear();

          day   = ('0' + day).slice(-2);
          month = ('0' + month).slice(-2);

          return `${year}-${month}-${day}`
    })
</script>

<script type="text/javascript">
import axios from 'axios'
export default {
  el: '#myform',
    data() {
        return{
          info: null,
          errored: false,
          loading: true,
          selected: '',
          name: null,
          app: null,
          email: null,
          cel: null,
          fecha: null,
        }
  },
  mounted () {
    axios
      .get('http://127.0.0.1:8000/api/servicios')
      .then(response => {
        this.info = response.data
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  },
  methods:{
    validateForm: function(e){
      if(!this.name){
        alert("Debes agregar el nombre!");
        e.preventDefault();
        }
      if(!this.app){
        alert("Debes agregar los apellidos!");
        e.preventDefault();
        }
      if(!this.email){
        alert("Debes agregar el email! Deben ser mas de 5 caracteres");
        e.preventDefault();
        }
      if(this.email && this.email.length <5){
        alert("El email debe ser mayor de 5 caracteres");
        e.preventDefault();
        }
      if(!this.cel){
        alert("Debes agregar el telefono!");
        e.preventDefault();
        }
      if(!this.fecha){
        alert("Debes elejir una fecha para reservar!");
        e.preventDefault();
        }
      if(!this.selected){
        alert("Debes elejir un servicio para reservar!");
        e.preventDefault();
        }
    }
  }
}
</script>