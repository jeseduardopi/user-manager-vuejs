<template>
  <div class="container mt-3">
    <AddUser v-on:handledata="handleUser" />
    <div class="row">
      <div class="col">
          <!-- <p class="text-success fw-bold">
            User Manager
            <router-link to="/users/add" class="btn btn-success btn-sm"
              ><i class="fa fa-plus-circle"></i> New</router-link
            >
          </p> -->
          <!-- <form>
            <div class="row">
            <div class="col-md-6">
              <input type="text" class="form-control" placeholder="Search Name" />
            </div>
            <div class="col">
              <input type="submit" class="btn btn-outline-dark" value="Search" />
            </div>
            </div>
          </form> -->
        </div>
      
      </div>
    </div>
  <!-- Spinner  -->
  <div v-if="loading">
    <div class="container">
      <div class="row">
        <div class="col">
          <Spinner />
        </div>
       </div>
    </div>
  </div>
<!-- Error Message -->
  <div v-if="!loading && errorMessage">
      <div class="container">
        <div class="row">
          <div class="col">
            <p class="h3 text-danger-fw-bold">{{ errorMessage }}</p>
          </div>
        </div>
      </div>
  </div>

  <div class="container mt-3" v-if="users.length > 0">
    <div class="row">
      <div class="col-md-6" v-for="user of users" :key="user">
        <div class="card my-2 list-group-item-success shadow-lg">
          <div class="card-body">
            <div class="row align-items-center">
              <div class="col-sm-4">
                <img src="../assets/585e4bcdcb11b227491c3396.png" alt="" class="img-fluid w-5"/>
              </div>
              <div class="col-sm-7">
                <ul>
                  <li class="list-group-item">Name : <span class="fw-bold">{{ user.name }}</span></li>
                  <li class="list-group-item">Email: <span class="fw-bold">{{ user.email }}</span></li>
                  <li class="list-group-item">Mobile : <span class="fw-bold">{{ user.phone }}</span></li>
                </ul>

              </div>
              <div class="col-sm-1 d-flex flex-column justify-content-center align-items-center">
                <router-link :to="`/users/view/${user.id}`" class="btn btn-warning my-1">
                  <i class="fa fa-eye"></i>                </router-link>
                  <router-link :to="`/users/edit/${user.id}`" class="btn btn-primary my-1">
                  <i class="fa fa-pen"></i> 
                </router-link>
                <button class="btn btn-danger my-1" @click="deleteUser(user.id)">
                  <i class="fa fa-trash"></i> 
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <pre>{{ newUsersArr }}</pre>

</template>

<script>
import Spinner from '@/components/Spinner.vue';
import { UserService } from '@/services/UserService';
import AddUser from './AddUser.vue';
export default {

    name: "UserManager",
    data: function () {
        return {
            newUsersArr: [],
            loading: false,
            users: [],
            errorMessage: null
        };
    },
    created: async function () {
        try {
            this.loading = true;
            let response = await UserService.getAllUsers();
            this.users = response.data;
            this.loading = false;
        }
        catch (error) {
            this.errorMessage = error;
            this.loading = false;
        }
    },
    // here not necesary method but used for learning purpose
    methods: {
    // getAllUsersData: async function() {
    //   return await UserService.getAllUsers();
      deleteUser : async function (userId) {

        this.users = this.users.filter((obj) => obj.id !== userId)

        // try {
        //   this.loading = true;
        //   let response = await UserService.deleteUser(userId);
        //   if(response) {
        //     let response = await UserService.getAllUsers();
        //     this.users = response.data;
        //     this.loading = false;
        //   } 
        // }
        // catch(error) {
        //   this.errorMessage = error;
        //     this.loading = false;
        // }
      },
      handleUser(user) {

        user.id = Math.floor(Math.random() * 1000);

        if(user) {
          let existingVal = this.users.find(item=>item.username === user.username)

          console.log(existingVal)
          this.users.push(user);
        
          user = "";
          
          

          // console.log(this.newUsersArr)
        }
      }
    },
    components: { Spinner, AddUser }
};
</script>

<style scoped></style>

