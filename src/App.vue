<template>
  <nav class="navbar navbar-expand-lg bg-light">
    <div class="container-fluid">
      <router-link class="navbar-brand" :to="{name:'Home'}">DKU19JAM</router-link>

      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <router-link class="nav-link active" :to="{name : 'Todos'}">Todos</router-link>
        </li>
      </ul>
    </div>
  </nav>
  <div class="container">
    <router-view/>
      <Toast></Toast>
  </div>
</template>

<script>
import Toast from '@/components/Toast'

export default {
  components: {
    Toast,
  },
}
</script>

<style scoped>

</style>
