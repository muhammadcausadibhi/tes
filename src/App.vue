<template>
  <NavbarComponent />
  <router-view />
</template>

<script>
import NavbarComponent from "./components/NavbarComponent";
export default {
  components:{
    NavbarComponent
  }
}
</script>
