<template>
  <Sidebar />
  <Home />
</template>

<script>
import Sidebar from './components/layouts/Sidebar.vue';
import Home from './pages/Home.vue';
export default {
  components: {
    Sidebar,
    Home
  }
}
</script>