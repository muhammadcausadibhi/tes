<template>
  <nav class="navbar navbar-expand-lg bg-dark navbar-dark shadow-md sticky-top">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">News</router-link>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <router-link class="nav-link active" aria-current="page" to="/"
              >Home</router-link
            >
          </li>
          <li class="nav-item dropdown">
            <router-link
              class="nav-link dropdown-toggle"
              to="/"
              role="button"
              data-bs-toggle="dropdown"
              aria-expanded="false"
            >
              Categories
            </router-link>
            <ul class="dropdown-menu">
              <li>
                <router-link
                  class="dropdown-item"
                  :to="{ name: 'category', params: { id: 'business' } }"
                >
                  Business
                </router-link>
              </li>
              <li>
                <router-link
                  class="dropdown-item"
                  :to="{ name: 'category', params: { id: 'health' } }"
                >
                  Health
                </router-link>
              </li>
              <li>
                <router-link
                  class="dropdown-item"
                  :to="{ name: 'category', params: { id: 'technology' } }"
                >
                  Technology
                </router-link>
              </li>
            </ul>
          </li>
        </ul>
        <form class="d-flex" role="search" @submit.prevent="searchCategory">
          <input
            class="form-control me-2"
            type="search"
            placeholder="Search"
            aria-label="Search category..."
            name="q"
            v-model="searchTerm"
          />
          <button class="btn btn-outline-success" type="submit">Search</button>
        </form>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "NavbarComponent",
  data() {
    return {
      searchTerm: "",
    };
  },
  methods: {
    searchCategory() {
      if (this.searchTerm) {
        this.$router.push(`/category/${this.searchTerm}`);
      }
    },
  },
};
</script>
