<template>
    <div class="container">
      <form @submit.prevent="login">
        <h2>Login</h2>
        <div class="input">
          <label for="email">Email address</label>
          <input
            class="form-control"
            type="text"
            name="email"
            placeholder="email@adress.com"
          />
        </div>
        <div class="input">
          <label for="password">Password</label>
          <input
            class="form-control"
            type="password"
            name="password"
            placeholder="password123"
          />
        </div>
        <div class="alternative-option">
          You don't have an account? <span @click="moveToRegister">Register</span>
        </div>
        <button type="submit" class="btn-pers" id="login_button">
          Login
        </button>
        <div
          class="alert"
          role="alert"
          id="alert_1"
        >
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import { getAuth, signInWithEmailAndPassword } from "firebase/auth";
  
  export default {
    data() {
      return {
        email: "",
        password: "",
      };
    },
    methods: {
      login(submitEvent) {
        this.email = submitEvent.target.elements.email.value;
        this.password = submitEvent.target.elements.password.value;
  
        const auth = getAuth();
        signInWithEmailAndPassword(auth, this.email, this.password)
          .then(() => {
            this.$router.push("/dashboard");
          })
          .catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
            console.log(errorCode);
            console.log(errorMessage);
            let alert_1 = document.querySelector("#alert_1");
            alert_1.classList.remove("d-none");
            alert_1.innerHTML = errorMessage;
            console.log(alert_1);
          });
      },
      moveToRegister() {
        this.$router.push("/register");
      },
    },
  };
  </script>

<style scoped>
.container {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

h2 {
  font-size: 24px;
  margin-bottom: 20px;
}

.input {
  margin-bottom: 20px;
}

.label {
  font-weight: bold;
}

.form-control {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.alternative-option {
  margin-top: 20px;
  font-size: 14px;
}

.alternative-option span {
  color: #007bff;
  cursor: pointer;
}

.btn-pers {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 4px;
  cursor: pointer;
}

.btn-pers:hover {
  background-color: #0056b3;
}

.alert {
  margin-top: 20px;
}

* {
  font-family: "Roboto";
}
</style>