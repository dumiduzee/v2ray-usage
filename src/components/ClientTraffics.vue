<template>
  <div>
    <!-- Navigation Bar -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark shadow-sm">
      <div class="container">
        <a class="navbar-brand" href="/">
          <img src="https://raw.githubusercontent.com/dumiduzee/private_images/main/images/Screenshot_1-removebg-preview.png?token=GHSAT0AAAAAACWLODCDHIWECAPEF2LOWI5YZWV2SIQ" alt="Native Shield" width="30" height="30" class="d-inline-block align-top">
          Native Shield
        </a>
        <button
          class="navbar-toggler"
          type="button"
          data-toggle="collapse"
          data-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav ml-auto">
            <li class="nav-item">
              <a class="nav-link" href="https://wa.me/+94751267252">Contact</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="https://chat.whatsapp.com/LlznmSUuhKNLSgn76wOAv2">Group</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="https://whatsapp.com/channel/0029Vaa2KYqEquiOtR26Zc3y">Channel</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="https://photos.app.goo.gl/oKzk4RSW7xGvCHPP9">Proofs</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <div class="container my-5">
      <!-- Total Data Usage Box -->
      <div class="total-usage-box p-4 mb-5 shadow text-center">
        <h3>Total Data Usage</h3>
        <p class="mb-0">{{ formatTraffic(totalTraffic) }}</p>
      </div>

      <!-- Individual Client Data Usage -->
      <div
        v-for="(traffic, index) in traffics"
        :key="traffic[0]"
        :class="['traffic-box', `gradient-${index % 2}`]"
        class="p-4 mb-4 shadow"
      >
        <h5 class="font-weight-bold">{{ traffic[3] }}</h5>
        <p class="mb-0">Traffic: {{ formatTraffic(traffic[4] + traffic[5]) }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      traffics: [],
      totalTraffic: 0
    };
  },
  mounted() {
    this.getTraffics();
  },
  methods: {
    async getTraffics() {
      try {
        const response = await axios.get('http://146.190.96.191:5000/api/client_traffics');
        this.traffics = response.data;
        this.calculateTotalTraffic();
      } catch (error) {
        console.error('Error fetching traffics:', error);
      }
    },
    calculateTotalTraffic() {
      this.totalTraffic = this.traffics.reduce((sum, traffic) => sum + parseInt(traffic[4]) + parseInt(traffic[5]), 0);
    },
    formatTraffic(bytes) {
      const gb = bytes / (1024 * 1024 * 1024);
      const tb = gb / 1024;

      if (tb >= 1) {
        return tb.toFixed(2) + ' TB';
      } else if (gb >= 1) {
        return gb.toFixed(2) + ' GB';
      } else {
        return (bytes / (1024 * 1024)).toFixed(2) + ' MB';
      }
    }
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');

body {
  font-family: 'Poppins', sans-serif;
}

.navbar {
  background-color: #343a40; /* Dark background */
}

.navbar-brand {
  color: #ffffff;
  font-weight: 600;
  display: flex;
  align-items: center;
}

.navbar-brand img {
  margin-right: 10px;
}

.navbar-nav .nav-link {
  color: #d4d4d4;
  margin-right: 20px;
  font-size: 1rem;
  font-weight: 500;
}

.navbar-nav .nav-link:hover {
  color: #ffffff;
}

.total-usage-box {
  border-radius: 10px;
  background-color: #e9ecef;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  font-size: 1.25rem;
  color: #495057;
}

.traffic-box {
  border-radius: 10px;
  padding: 15px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  color: #343a40;
}

/* Gradient Backgrounds */
.gradient-0 {
  background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
  color: #fff;
}

.gradient-1 {
  background: linear-gradient(135deg, #ff9966 0%, #ff5e62 100%);
  color: #fff;
}

/* Ensure responsiveness */
.container {
  max-width: 600px;
}

@media (max-width: 768px) {
  .navbar-nav {
    text-align: center;
  }

  .navbar-nav .nav-link {
    margin-right: 0;
    margin-bottom: 10px;
  }

  .total-usage-box,
  .traffic-box {
    padding: 10px;
    font-size: 1rem;
  }

  /* Add margin for mobile screens to avoid touching edges */
  .container {
    /* margin-left: 15px; */
    margin-right: 15px;
  }
}
</style>
