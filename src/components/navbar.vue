<template>
  <div id="navbar">
    <b-navbar toggleable="md" type="dark" variant="info">

      <b-nav-toggle target="nav_collapse"></b-nav-toggle>

      <img src="../assets/logo.png">

      <b-navbar-brand href="/" style="margin-left: 15px;">Home</b-navbar-brand>

      <b-collapse is-nav id="nav_collapse">

        <!-- Right aligned nav items -->
        <b-nav b-navbar-nav class="ml-auto">
          <b-nav b-navbar-nav>
            <b-nav-item href="#/CallEmployee">Abrir Caja</b-nav-item>
            <b-nav-item href="#/">Cola</b-nav-item>
            <b-nav-item href="#/add">Cliente Nuevo</b-nav-item>
          </b-nav>
        </b-nav>

      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
  export default {
    name: 'navbar'
  }
</script>

<style scoped>
  img {
    height: 50px;

  }

  a {
    color: #ffffff !important;
  }
  nav {
    border-radius: 4px 4px 0px 0px;
  }
</style>

