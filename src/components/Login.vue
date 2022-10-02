<template>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
  <div class="container-md">
    <br>
    <h1>Consulta las reservaciones</h1>
    <form action class="form" id="myform" @submit.prevent="login">
      <img alt="Vue logo" src="https://salonyspasv.com/wp-content/uploads/2022/09/new_logo.jpeg" width="250">

      <br>
      <label class="title" for="#email">Correo:</label>
      <input v-model="email" class="form-control" type="email" id="email" placeholder="Email">
      <br>

      <label class="title" for="#password">Password:</label>
      <input v-model="password" class="form-control" type="password" id="password" placeholder="Password">
      <br>

      <p>
        <input type="submit" class="btn btn-outline-success" value="Iniciar Sesion" >
      </p>
    </form>
  </div>
</template>

// <script type="text/javascript">
// import axios from 'axios'
// export default {
//     el: '#myform',
//       data(){
//         return{
//           email:'',
//           password:''
//         }
//       },
//       methods:{
//         login: function(e){
//           e.preventDefault()
//           if(!this.email){
//         return this.$swal({
//         toast: true,
//         position: 'bottom-end',
//         showConfirmButton: false,
//         timer: 3000,

//         icon: 'error',
//         title: 'Oops...',
//         text: 'Debes poner un correo!',
//       });
//         }

//         if(!this.password){
//         return this.$swal({
//         toast: true,
//         position: 'bottom-end',
//         showConfirmButton: false,
//         timer: 3000,

//         icon: 'error',
//         title: 'Oops...',
//         text: 'Debes poner una contraseña!',
//       });
//         }

//         }
//       }
// }
// </script>

