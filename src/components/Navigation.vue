<template>
  <div>
    <nav class="navbar navbar-expand bg-primary navbar-dark">
      <div class="container-fluid">
        <router-link class="navbar-brand" to="/">CuppaCoffee</router-link>
        <div class="navbar-nav ml-auto">
          <router-link class="nav-item nav-link" to="/login">Login</router-link>
          <router-link class="nav-item nav-link" to="/register"
            >Register</router-link
          >
          <router-link class="nav-item nav-link" to="/groups"
            >Groups</router-link
          >
        </div>
      </div>
    </nav>
  </div>
</template>
