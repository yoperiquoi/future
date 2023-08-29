<template>
  <div>
    <button id="sign_out" @click="signOut">
      Logout
    </button>
    <div class="flex-container">
      <div class="left-column">
        <h2>Add Key-Value Data</h2>
        <form @submit.prevent="submitData">
          <div class="form-group">
            <label for="key">Key:</label>
            <input type="text" id="key" v-model="key" required>
          </div>
          <div class="form-group">
            <label for="value">Value:</label>
            <input type="text" id="value" v-model="value" required>
          </div>
          <button type="submit">Submit</button>
        </form>
        <h2>Key-Value Data</h2>
        <table class="custom-table">
          <thead>
            <tr>
              <th>Key</th>
              <th>Value</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in items" :key="index">
              <td>{{ item.key }}</td>
              <td>{{ item.value }}</td>
            </tr>
          </tbody>
        </table>
    </div>
    <div class="right-column">
      <h2>Bar Chart</h2>
      <Bar :data="barChartData" :options="chartOptions" />
    </div>
  </div>
  </div>
</template>

<script>
import { getAuth } from "firebase/auth";
import { initializeApp } from "firebase/app";
import { getFirestore, collection, addDoc, getDocs } from "firebase/firestore";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
} from 'chart.js'
import { Bar } from 'vue-chartjs'

ChartJS.register(CategoryScale, LinearScale, BarElement, Title, Tooltip, Legend)


const firebaseConfig = {
  apiKey: process.env.VUE_APP_FIREBASE_API_KEY,
  authDomain: process.env.VUE_APP_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.VUE_APP_FIREBASE_PROJECT_ID,
  storageBucket: process.env.VUE_APP_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.VUE_APP_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.VUE_APP_FIREBASE_APP_ID
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

const auth = getAuth();

export default {
  components: {
    Bar
  },
  data() {
    return {
      email: auth.currentUser.email,
      key: "",
      value: "",
      items: [],
      barChartData: {
          labels: [""],
          datasets: [
            {
              label: '',
              backgroundColor: '',
              data: []
            }
          ]
      },
      chartOptions: {
          responsive: true
      }
    };
  },
  methods: {
    signOut() {
      auth
        .signOut()
        .then(() => {
          console.log("Sign Out completed");
          this.$router.push("/");
        })
        .catch((error) => console.log(error));
    },
    async submitData() {
      const data = {
        key: this.key,
        value: this.value,
      };

      addDoc(collection(db, "data"), data)
        .then(() => {
          console.log("Data added to Firestore");
          this.key = "";
          this.value = "";
        })
        .catch((error) => {
          console.error("Error adding data to Firestore: ", error);
        });
        await this.fetchData();
    },
    async fetchData() {
      try {
        const querySnapshot = await getDocs(collection(db, "data"));
        const data = [];
        const barData = {
          labels: [],
          datasets: [
            {
              label: '',
              backgroundColor: '',
              data: []
            }
          ]
        }
        querySnapshot.forEach((doc) => {
          data.push(doc.data());
        });
        this.items = data;
        data.forEach((item) => {
          barData.labels.push(item.key);
          barData.datasets[0].data.push(Number(item.value));
        });
        this.barChartData = barData;
          } catch (error) {
            console.error("Error fetching data from Firestore: ", error);
          }
    },
  },
  created() {
    this.fetchData();
  }
};
</script>

<style scoped>
.flex-container {
  display: flex;
  justify-content: space-between;
  padding: 20px;
}

.left-column {
  flex: 1;
  padding: 20px;
  background-color: #f7f7f7;
  border: 1px solid #ddd;
  border-radius: 5px;
}

form {
  margin-bottom: 20px;
}

.form-group label {
  font-weight: bold;
}

.form-group input[type="text"] {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
  border: 1px solid #ddd;
  border-radius: 3px;
}

button[type="submit"] {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 3px;
  margin-top: 5px;
}

.custom-table {
  border-collapse: collapse;
  width: 100%;
  border: 1px solid #ddd;
  margin-top: 20px;
}

.custom-table th {
  background-color: #f2f2f2;
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.custom-table td {
  padding: 8px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.right-column {
  flex: 1;
  padding: 20px;
  background-color: #f7f7f7;
  border: 1px solid #ddd;
  border-radius: 5px;
}

canvas {
  width: 100%;
  height: auto;
}

#sign_out {
  margin-top: 10px;
  background-color: #dc3545; 
  color: #fff; 
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 3px;
  transition: background-color 0.3s; 
}


#sign_out:hover {
  background-color: #c82333;
}

* {
  font-family: "Roboto";
}
</style>