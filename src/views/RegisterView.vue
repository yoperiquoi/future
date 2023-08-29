<template>
    <div class="container">
      <form @submit.prevent="register">
        <h2>Register</h2>
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
          Already have an account? <span @click="moveToLogin">Login</span>
        </div>
  
        <button type="submit" id="register_button" class="btn-pers">
          Register
        </button>
        <div
          class="alert"
          role="alert"
          id="alert_2"
        >
        </div>
      </form>
    </div>
  </template>
  
  <script>
  import { getAuth, createUserWithEmailAndPassword } from "firebase/auth";
  
  export default {
    data() {
      return {
        email: "",
        password: "123456",
      };
    },
    methods: {
      register(submitEvent) {
        this.email = submitEvent.target.elements.email.value;
        this.password = submitEvent.target.elements.password.value;
  
        const auth = getAuth();
        createUserWithEmailAndPassword(auth, this.email, this.password)
          .then((userCredential) => {
            const user = userCredential.user;
            console.log(user);
            console.log("Registration completed");
            this.$router.push("/");
          })
          .catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
            console.log(errorCode);
            console.log(errorMessage);
            let alert_2 = document.querySelector("#alert_2");
            alert_2.classList.remove("d-none");
            alert_2.innerHTML = errorMessage;
            console.log(alert_2);
          });
      },
      moveToLogin() {
        this.$router.push("/");
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