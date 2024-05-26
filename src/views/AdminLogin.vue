<template>
  <div class="container">

    <div class="login-form-container">
      <div id="login">
        <h1 class="title">Accounting Management System</h1>
        <p class="subtitle">Sign in to continue</p>
        <div class="input-group">
          <label class="inputTitle" for="email">Email</label>
          <input type="text" id="email" v-model="email" required>
        </div>
        <div class="input-group">
          <label class="inputTitle" for="password">Password</label>
          <input type="password" id="password" v-model="password" required>
        </div>
        <button @click="login">Log in</button>
        <p>{{ errorMessage }}</p>
      </div>
    </div>

    <div class="trapezoid"></div>
  </div>
</template>

<script>
 import axios from 'axios';

export default {
  data() {
    return {
      email: '',
      password: '',
      errorMessage: ''
    };
  },
  methods: {
    async login() {
      try {
        const csrfToken = document.querySelector('meta[name="csrf-token"]').getAttribute('content');
        
        const response = await axios.post('http://127.0.0.1:8000/login', {
          email: this.email,
          password: this.password
        }, {
          headers: {
            'X-CSRF-TOKEN': csrfToken
          }
        });

        // Check if authentication was successful
        if (response.status === 200) {
          // Redirect to admin panel
          this.$router.push('/employee');
        } else {
          // Display error message
          this.errorMessage = 'Invalid email or password';
        }
      } catch (error) {
        // Display error message
        this.errorMessage = 'An error occurred. Please try again later.';
        console.error('Login failed:', error);
      }
    }
  }
};
</script>

<style scoped>
.container {
  margin-top: 170px;
  display: flex;
  justify-content: space-between; /* Align children with space between them */
  align-items: center;
  padding: 0 200px; /* Add padding to the container */
}

.login-form-container {
  max-width: 500px; /* Adjust the max-width as needed */
  padding: 20px;
  background-color: #E3F1F8; /* Light blue background */
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1); /* Soft shadow */
  margin-left: 340px;
}

.input-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
  color: #B3D9E6; /* Light blue */
}

input[type="text"],
input[type="password"] {
  width: 100%; /* Adjust the width as needed */
  height: 35px;
  padding: 10px;
  border: 1px solid #B3D9E6; /* Light blue border */
  background-color: #E3F1F8; /* Light blue background */
}

button {
  width: 100%; /* Adjust the width as needed */
  background-color: #92d1e7; /* Light blue */
  color: black; /* White */
  border: none;
  padding: 5px;
  border-radius: 5px;
  cursor: pointer;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  font-weight: bold;
  font-size: 20px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #207BA6; /* Darker blue on hover */
  color:white;
}

.subtitle {
  color: rgb(53, 53, 53); /* Light blue */
  margin: 0;
  font-size: 15px;
  text-align: center;
}

.inputTitle {
  color: rgb(53, 53, 53); /* Light blue */
  text-align: left;
  margin-left: 0px;
}

.title {
  font-weight: bold;
  margin: 0;
  font-size: 40px;
  color: rgb(53, 53, 53); /* Light blue */
  text-align: center;
}

.trapezoid {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 70px;
  background-color: #B3D9E6; /* Light blue */
  align-items: center;
}

</style>
