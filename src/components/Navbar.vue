<template>
  <nav class="navbar">
    <ul>
      
      <img src= "/OnlyAiArtLogo.png" @click="goToHomePage">

      <div id = "search-bar-container">
        <div id = "icon-container">
          <img src="@/assets/search.svg" id="search-icon" alt="Search">
        </div>
        <input id = "search-box" type="text" placeholder="Dog flying in a jelly sky images...">
      </div>
      
      <div id = "left-container">
        
        <div id = "profile-image-container" @click="goToProfilePage" v-if="userLoggedIn">
          <img :src = "profileUrl">
        </div>
        
        <button id = "login-button" type="button" @click="goToLoginPage" v-if="!userLoggedIn">Log in</button>
        <button id = "register-button" type="button" @click="goToRegisterPage" v-if="!userLoggedIn">Sign up</button>
      </div>


    </ul>
  </nav>
</template>

<script>
  import axiosObj from '../axios-config';



  export default {

    name: 'Navbar',

    data() {
      return {
        userLoggedIn: false,
        userId: null,

      }
    },

    async mounted() {
      await this.checkLoginStatus()
      this.$emit('login-status-checked')
    },

    computed: {

    profileUrl(){

        
        return "this.response.data.profilePhotoUrl"
    
    }

    },


    methods: {


      async checkLoginStatus() {
        try {

          
          const response = await axiosObj.get('/user');
          
          if(response.data.userId === null){

            userLoggedIn = false;

            console.log(false)

          }else{

            this.userId = response.data.userId;
            
            this.userLoggedIn = true;
          }

        } catch (error) {
          console.error('Error fetching posts:', error)
        }
      },

      goToProfilePage() {
        this.$router.push(`/profile/${this.userId}`);
      },

      goToHomePage() {
        this.$router.push('/');
      },

      goToLoginPage() {
        this.$router.push('/login');
      },

      goToRegisterPage() {
        this.$router.push('/register');
      }
    }

  }
</script>

<style scoped>

.navbar {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #000000;
  padding: 0.8rem;
  border-color: #4D4D4D;
  border-width: 0px 0px 2px 0px; 
  border-style: solid;
  box-sizing: border-box;
  z-index: 1000;

}
.navbar ul {
  list-style: none;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  gap: 10px;
  padding-left: 30px;
  padding-right: 20px;
}

.navbar img {
  width: 45px; 
  height: auto; 
  cursor: pointer;
}

#profile-image-container{
  width: 45px;
  height: 45px;
  border-radius: 100px;
  background-color: gray;
  cursor: pointer;
}


#left-container{
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 20px;
}


#post-button {

  background-color: #0F0F0F;
  color: white;
  border-radius: 100px;
  border: 1px solid rgba(107, 107, 107, 0.3);
  width: 125px;
  height: 45px;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  font-weight: 800;
  font-size: 22px;
  transition: background-color 0.2s ease;
}

#login-button{

  background-color: #0F0F0F;
  color: white;
  border-radius: 10px;
  border: 2px solid rgba(107, 107, 107, 0.3);
  width: 125px;
  height: 45px;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  font-weight: 800;
  font-size: 15px;
}

#register-button{

  background-color: #0F0F0F;
  color: white;
  border-radius: 10px;
  border: 2px solid #FF5C5C;
  width: 125px;
  height: 45px;
  cursor: pointer;
  font-family: 'Inter', sans-serif;
  font-weight: 800;
  font-size: 15px;
}

#search-bar-container{

  position: fixed;
  width: 40%;
  height: 50px;
  padding-left: 10px;
  background-color: var(--tertiary-color);
  border-radius: 10px;
  border: 1px solid #222222;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: start;

}

#icon-container{
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding-right: 10px;
}

#search-icon{
  height: 30px;
  width: 30px;
}

#search-box{
  width: 100%;
  height: 100%;
  background-color: var(--bg-color);
  color: white;
  border: none;
  text-align: center;
  font-family: 'Inter', sans-serif;
  font-weight: 800;
  font-size: 22px;
  border-left: 1px solid #222222;
}

input:focus {
  outline: none;
  box-shadow: none;
}


.navbar li a {
  color: white;
  text-decoration: none;
}
</style>
