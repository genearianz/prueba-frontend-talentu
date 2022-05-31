<template>
    <div class="container mt-5">

        <div class="alert alert-dismissible fade show" :class="alertClass" role="alert" v-if="alert">
            {{ this.alertMessage }}
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close" v-on:click="closeAlert"></button>
        </div>

        <div class="row">

            <!-- REGISTRAR UN USUARIO NUEVO -->
            <div class="col-md-3 mb-3">
                <div class="card">
                    <div class="card-header">
                        Agregar Usuario
                    </div>
                    <div class="card-body">
                        
                        <form v-on:submit="addNewUser">
                            <div class="mb-3 text-start">
                                <label class="form-label">Nombre</label>
                                <input 
                                    type="text" 
                                    class="form-control" 
                                    v-model="user.first_name" 
                                    placeholder="Nombre">
                            </div>
                            <div class="mb-3 text-start">
                                <label class="form-label">Apellido</label>
                                <input 
                                    type="text" 
                                    class="form-control" 
                                    v-model="user.last_name" 
                                    placeholder="Apellido">
                            </div>
                            <div class="mb-3 text-start">
                                <label class="form-label">Correo</label>
                                <input 
                                    type="email" 
                                    class="form-control" 
                                    v-model="user.email" 
                                    placeholder="Correo">
                            </div>
                            <div class="mb-3 text-start">
                                <label class="form-label">Fecha de Nacimiento</label>
                                <input 
                                    type="date" 
                                    class="form-control" 
                                    v-model="user.birthday" 
                                    placeholder="Fecha de Nacimiento">
                            </div>
                            
                            <button v-on:click="addNewUser" class="btn btn-primary w-100">Agregar Usuario</button>
                        </form>

                    </div>
                </div>
            </div>

            <!-- MOSTRAR USUARIOS -->
            <div class="col-md-9 mb-3">
                <div class="card">
                    <div class="card-body table-responsive">
                        <table class="table">
                            <thead class="table-dark">
                                <tr>
                                <th scope="col">ID</th>
                                <th scope="col">Nombre</th>
                                <th scope="col">Edad</th>
                                <th scope="col">Email</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="user, index in users" v-bind:key="index">
                                <td>{{ user.id }}</td>
                                <td>{{ user.first_name }} {{ user.last_name }}</td>
                                <td>{{ user.birthday }}</td>
                                <td>{{ user.email }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {  
  name: 'HelloWorld',
  data() {
    return {
      users: null,      
      user: {
          first_name: '',
          last_name: '',
          email: '',
          birthday: ''
      },
      alert: false,
      name: '',
      alertClass: '',
      alertMessage: ''
    }
  },
  created(){

        let dataLocal = localStorage.getItem('users');

        if(dataLocal != null ){
            this.users = JSON.parse(dataLocal); 
        } else {
            this.getUsers();
        }

  },
  methods: {
    getUsers() {
        axios.get('https://reqres.in/api/users?page=1')
            .then( res => {
                    this.users = res.data.data;
                    })
            .catch( e => console.log(e));

            console.log('asd')
    },
    addNewUser(e){
        e.preventDefault();
        let id = this.users.length+1;
        let date = new Date().getFullYear();
        let birthday = new Date(this.user.birthday).getFullYear();
        let age = date - birthday;

        if(this.user.first_name && this.user.last_name && this.user.email && this.user.birthday) {

            this.users.push({
                id: id,
                first_name: this.user.first_name,
                last_name: this.user.last_name,
                birthday: age,
                email: this.user.email,
            });
            
            this.alertClass = "alert-success";
            this.alertMessage = "Se agrego el usuario: " + this.user.first_name + " " + this.user.last_name + " correctamente!";
            this.alert = true;
            this.name = this.user.first_name + " " + this.user.last_name;
            this.user = {
                first_name: "",
                last_name: "",
                birthday: "",
                email: "",
            }

            this.localStorage();

        } else {
            this.alertClass = "alert-danger";
            this.alertMessage = "Error al agregar usuario. Por favor intente nuevamente y asegurese de rellenar los campos."
            this.alert = true;   
        }
    },
    localStorage(){
        localStorage.setItem('users', JSON.stringify(this.users));
    },
    closeAlert(){
        return this.alert = false;
    }
  },
  mounted(){
    //this.getUsers();
  }
}
</script>