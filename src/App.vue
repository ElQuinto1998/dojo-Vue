<template>
  <div id="app" class="container">

    <h1>Links Académicos</h1>

    <div class="card">

      <!-- Creación del formulario -->
      <div class="card-header">
        <h3>Agregar nuevo link</h3>
      </div>

      <div class="card-body">

        <!-- Agrupar label con input -->
        <!-- submit.prevent previene que la página se actualice -->
        <!-- Crear formulario -->
        <form v-on:submit.prevent="aggLink" class=f orm-inline>
          <div class="form-group">
            <label> Título: </label>

            <input class="form-control" v-model="newLink.title" placeholder="Título" type="text">
          </div>
          <div class="form-group">
            <label> Autor: </label>

            <input class="form-control" v-model="newLink.author" placeholder="Autor" type="text">
          </div>
          <div class="form-group">
            <label> Url: </label>

            <input class="form-control" v-model="newLink.url" placeholder="Url" type="text">
          </div>

          <input type="submit" class="btn btn-success" value="Agregar nuevo">
        </form>
      </div>

    </div>

    <hr>

    <!-- Crear tabla -->
    <div class="card">
      <div class="card-header">
        <h3 class="card-title">Lista de enlaces</h3>
      </div>

      <div class="card-body">
        <table class="table table-striped">
          <!-- Encabezado -->
          <thead>
            <tr>
              <th>Título</th>
              <th>Autor</th>
              <th>Borrar</th>
            </tr>
          </thead>
          <!-- Cuerpo-->
          <tbody>
            <tr v-for = "link in links">
              <td>
                <a target="_blank" v-bind:href="link.url">{{ link.title}}</a>
              </td>
              <td>
                {{link.author}}
              </td>
              <td>
                <button v-on:click="deleteLink(link)" class="btn btn-danger">
                  <i class="fas fa-trash-alt"></i>
                </button>
              </td>
            </tr>
          </tbody>
        </table>

      </div>
    </div>


  </div>
</template>

<script>
  import Firebase from "firebase";
  import toastr from "toastr";

  let config = {
    apiKey: "AIzaSyDUe-Ftbc8OyA1zEO-LDCRtm9eEmhaHZxU",
    authDomain: "dojovue.firebaseapp.com",
    databaseURL: "https://dojovue.firebaseio.com",
    projectId: "dojovue",
    storageBucket: "dojovue.appspot.com",
    messagingSenderId: "292909581145"
  };

  //Conexion a BD
  let app = Firebase.initializeApp(config);
  let db = app.database();

  let linksRef = db.ref("links");

  export default {
    name: "App",
    firebase: {
      links: linksRef
    },
    //Crear datos
    data() {
      return {
        newLink: {
          title: "",
          author: "",
          url: ""
        }
      };
    },
    methods: {
      aggLink: function () {
        //Guardar datos
        linksRef.push(this.newLink);
        this.newLink.title = "";
        this.newLink.author = "";
        this.newLink.url = "";
      },
      //Borrar link
      deleteLink: function (link) {
        linksRef.child(link[".key"]).remove();
        toastr.success("Link eliminado");
      }
    }
  };
</script>

<style>
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>