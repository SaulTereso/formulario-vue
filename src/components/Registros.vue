<template>
    <h1>CRUD</h1>
    <div id="app">
  {{ info }}
</div>
</template>

<script type="text/javascript">
import axios from 'axios'
export default {
    el:'#app',
    data(){
        return{
            info:null,
        }
    },
    mounted(){
        axios
        .get('http://127.0.0.1:8000/api/citas')
        .then(response => (this.info = response))
    }
}
</script>