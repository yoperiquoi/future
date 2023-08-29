<template>
  <div class="background"></div>
  <div class="navbar">
    <nav class="nav">
      <div id="name_project">
        POC project
      </div>
    </nav>
  </div>
  <router-view id="content" />
</template>

<style scoped>
.background {
  background-color: #f0f0f0;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
}

.navbar {
  background-color: #007bff;
  color: white;
  height: 60px;
  display: flex;
  align-items: center;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
}

.nav {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 0 20px;
}

#name_project {
  font-size: 20px;
  font-weight: bold;
}

#content {
  margin-top: 60px;
  padding: 20px;
}

@font-face {
  font-family: "Roboto";
  src: local("Roboto"),
   url(./fonts/Roboto-Regular.ttf) format("truetype");
}

* {
  font-family: "Roboto";
}
</style>