<template>
  <nav class="navbar navbar-expand-lg bg-light">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/" >DKU19JAM</router-link>

        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <router-link class="nav-link active" to="/todos">Todos</router-link>
          </li>
        </ul>
      </div>
  </nav>
  <router-view></router-view>
</template>

<script>

export default {

}
</script>

<style>

</style>
