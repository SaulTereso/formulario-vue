<template> 
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
  <div class="container-md">
    <br>
    <h1>Reserva tu cita</h1>
      <img alt="Vue logo" src="https://salonyspasv.com/wp-content/uploads/2022/09/new_logo.jpeg" width="250">
        <form id="myform" @submit.prevent="validateForm" method="POST">
          
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
              <input v-model="fecha" type="date" :min="minDate" :max="maxDate" class="form-control">
            </div>

            <div class="col">
              <label for="time">Elija la hora para su reservación</label>
              <!-- <input v-model="time" type="time" class="form-control" min="09:00" max="20:00"> -->
              <input v-model="time" type="time" list="popularHours" class="form-control" />
              <datalist id="popularHours">
                <option value="09:00"></option>
                <option value="09:30"></option>
                <option value="10:00"></option>
                <option value="10:30"></option>
                <option value="11:00"></option>
                <option value="11:30"></option>
                <option value="12:00"></option>
                <option value="12:30"></option>
                <option value="13:00"></option>
                <option value="13:30"></option>
                <option value="14:00"></option>
                <option value="14:30"></option>
                <option value="15:00"></option>
                <option value="15:30"></option>
                <option value="16:00"></option>
                <option value="16:30"></option>
                <option value="17:00"></option>
                <option value="17:30"></option>
                <option value="18:00"></option>
                <option value="18:30"></option>
                <option value="19:00"></option>
                <option value="19:30"></option>
                <option value="20:00"></option>
              </datalist>
            </div>
          </div>
          <br>
          
          <!-- Iconos para los mensajes de alerta  -->

          <!-- <svg xmlns="http://www.w3.org/2000/svg" style="display: none;">
          <symbol id="check-circle-fill" viewBox="0 0 16 16">
            <path d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zm-3.97-3.03a.75.75 0 0 0-1.08.022L7.477 9.417 5.384 7.323a.75.75 0 0 0-1.06 1.06L6.97 11.03a.75.75 0 0 0 1.079-.02l3.992-4.99a.75.75 0 0 0-.01-1.05z"/>
          </symbol>
          
          <symbol id="exclamation-triangle-fill" viewBox="0 0 16 16">
            <path d="M8.982 1.566a1.13 1.13 0 0 0-1.96 0L.165 13.233c-.457.778.091 1.767.98 1.767h13.713c.889 0 1.438-.99.98-1.767L8.982 1.566zM8 5c.535 0 .954.462.9.995l-.35 3.507a.552.552 0 0 1-1.1 0L7.1 5.995A.905.905 0 0 1 8 5zm.002 6a1 1 0 1 1 0 2 1 1 0 0 1 0-2z"/>
          </symbol>
          </svg> -->

          <!-- Fin de los iconos para los mensajes de alerta  -->


          <!--Select con API-->
          <div class="col">
            <label for="service">Nuestros Servicios: </label>
            <section v-if="errored" class="alert alert-danger align-items-center">
              <!-- <svg class="bi flex-shrink-0 me-2"><use xlink:href="#exclamation-triangle-fill"/></svg> -->
              <strong>No es posible obtener la información en este momento, por favor recargue nuevamente o intente más tarde</strong>
            </section>

            <section v-else>
              <div v-if="loading" class="alert alert-success align-text-center">
                <!-- <svg class="bi flex-shrink-0 me-2"><use xlink:href="#check-circle-fill"/></svg> -->
                <strong>Cargando Información...</strong></div>
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
            <input type="submit" class="btn btn-outline-success col-sm-2 m-3" value="Reservar" >
          </p>
          <!--Fin prueba de validacion del formulario-->
          
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
          name: '',
          app:  '',
          email:'',
          cel:  '',
          fecha:'',
          time: '',
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

      //!Validación si hay campos vacios ("Alertas")
      e.preventDefault()
      if(!this.name){
        //  return alert("Debes agregar el nombre!");
        return this.$swal({
        toast: true,
        position: 'bottom-end',
        showConfirmButton: false,
        timer: 3000,

        icon: 'error',
        title: 'Oops...',
        text: 'Debes agregar el nombre!',
      });
        }
      if(!this.app){
        // alert("Debes agregar los apellidos!");
        return this.$swal({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 3000,

        icon: 'error',
        title: 'Oops...',
        text: 'Debes agregar los apellidos!',
      });
        }
      if(!this.email){
        // alert("Debes agregar el email! Deben ser mas de 5 caracteres");
        return this.$swal({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 3000,

        icon: 'error',
        title: 'Oops...',
        text: 'Debes agregar el email! Deben ser mas de 5 caracteres',
      });
        }
      if(this.email && this.email.length <5){
        // alert("El email debe ser mayor de 5 caracteres");
        return this.$swal({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 3000,

        icon: 'error',
        title: 'Oops...',
        text: 'El email debe ser mayor de 5 caracteres',
      });
        }
      if(!this.cel){
        //alert("Debes agregar el telefono!");
        return this.$swal({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 3000,

        icon: 'error',
        title: 'Oops...',
        text: 'Debes agregar el telefono!',
      });
        }
      if(!this.fecha){
        //alert("Debes elejir una fecha para reservar!");
        return this.$swal({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 3000,

        icon: 'error',
        title: 'Oops...',
        text: 'Debes elejir una fecha para reservar!',
      });
        }
      if(!this.time){
        // alert("Debes elejir un horaio para continuar!");
        return this.$swal({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 3000,

        icon: 'error',
        title: 'Oops...',
        text: 'Debes elejir un horaio para continuar!',
      });
        }
      if(!this.selected){
        // alert("Debes elejir un servicio para reservar!");
        return this.$swal({
        toast: true,
        position: 'top-end',
        showConfirmButton: false,
        timer: 3000,

        icon: 'error',
        title: 'Oops...',
        text: 'Debes elejir un servicio para reservar!',
      });
        }
      // let url = "http://127.0.0.1:8000/api/citas"; 
      // axios.post(url, data)
      // .then(() => {
      //   this.$swal({
      //     title: '¿Estas seguro de que quieres agendar est cita?',
      //     icon: 'question',
      //     showConfirmButton: true,
      //     ConfirmButtonText: 'Si confirmar!',
      //     ShowCancelButton: true,
      //     CancelButtonText: 'no, cancelar'
      //   }).then((result) => {
      //     if(result.value){
      //       this.$swal({
      //         title: 'Agendado',
      //         text: 'Tu cita se ah agendado correctamente',
      //         icon:'success'
      //       }).then((result) => {
      //         if(result.value){
      //         console.log('entra el ok')
      //         this.selected = '' ;
      //         this.name = '';
      //         this.email= '';
      //         this.app = '';
      //         this.time = '';
      //         this.cel = '';
      //         this.fecha = '';
      //         }
      //       })
      //     }
      //   })
      // }).catch((error) => {
      //          console.log(error)
      //     this.failed = true
      // })
//       const saveCita = () => {
//         this.$swal({
//           title: "¿Estas seguro?",
// text: "¿Estas seguro que quieres agendar esta cita?",
// icon: "warning",
// showCancelButton: true,
// confirmButtoncolor:"#3085d6",
// cancelButtonColor: "#d33",
//  confirmButtonText: "Si, agendar", 
//  cancelButtonText: "No, cancelar",
//         }).then((result) => {
//           if(result.value){
//             let url = "http://127.0.0.1:8000/api/citas";
//             axios.post(url)
//             .then(( ) => {
//               this.name= this.name,
//           this.app= this.app,
//           this.email= this.email,
//           this.cel= this.cel,
//           this.fecha= this.fecha,
//           this.horario= this.time,
//           this.selected= this.selected
//             }).then(() => {
//               this.$swal({
//                 title: 'Agendado',
//                 text: 'Tu cita se agendo correctamente',
//                 icon: 'success',
//               }).then((result) => {
//                 if(result.value){
//                                 console.log('entra el ok')
//               this.selected = '' ;
//               this.name = '';
//               this.email= '';
//               this.app = '';
//               this.time = '';
//               this.cel = '';
//               this.fecha = '';
//                 }
//               })
//             })
//           }
//         }).catch((error) => {
//           console.log(error);
//         })
//       }
      axios.post('http://127.0.0.1:8000/api/citas', {
          name: this.name,
          app: this.app,
          email: this.email,
          cel: this.cel,
          fecha: this.fecha,
          horario: this.time,
          selected: this.selected,
        }).then(()=>{
          this.$swal({
            icon: 'success',
            title: 'Bien hecho',
            text: 'Cita registrada con exito!',
          }).then((result) => {
            if(result.value) {
              console.log('entra el ok')
              //!Reset del formulario
              this.selected = '' ;
              this.name = '';
              this.email= '';
              this.app = '';
              this.time = '';
              this.cel = '';
              this.fecha = '';
            }
          });
        })
        .catch(error => {
          this.$swal({
            title: 'Error',
            text: error.response.data.message,
            icon: 'error',
          })
          console.log(error.response.data.message)
          this.failed = true
      });
      
    }
  }
}
</script>