<template>
  <div class="container my-4">
    <h1 id="heading">
      {{
        this.$route.params.id.charAt(0).toUpperCase() +
        this.$route.params.id.slice(1)
      }}
    </h1>
    <div v-if="isLoading" class="my-4">
      <div class="text-center">
        <div class="spinner-border" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
      </div>
    </div>
    <div v-else-if="articles.length > 0">
      <div class="row my-4" id="news">
        <div
          class="col-sm-4 mb-3"
          v-for="article in articles"
          :key="article.id"
        >
          <div class="card mx-auto">
            <img
              class="card-img-top"
              :src="article.urlToImage"
              alt="image not available"
            />
            <div class="card-body">
              <h5 class="card-title">{{ article.title }}</h5>
              <p class="card-text">{{ article.description }}</p>
              <a target="_blank" :href="article.url" class="btn btn-primary"
                >Read More</a
              >
            </div>
          </div>
        </div>
      </div>

      <nav class="container">
        <ul class="pagination justify-content-between">
          <li class="page-item">
            <a
              class="page-link"
              @click="fetchPreviousPage"
              :class="{ disabled: isPreviousDisabled }"
              >Previous</a
            >
          </li>

          <li class="page-item">
            <a
              class="page-link"
              @click="fetchNextPage"
              :class="{ disabled: isNextDisabled }"
              >Next</a
            >
          </li>
        </ul>
      </nav>
    </div>

    <div v-else>
      <AlertComponent v-bind:msg="errorMsg" />
    </div>
  </div>
</template>
<script>
import AlertComponent from "../components/AlertComponent.vue";

export default {
  name: "CategoryView",
  components: {
    AlertComponent,
  },
  data() {
    return {
      articles: [],
      currentPage: 1,
      totalPages: null,
      isLoading: true,
      errorMsg: null,
    };
  },
  watch: {
    $route: {
      immediate: true,
      handler() {
        this.fetchData();
      },
    },
  },
  mounted() {
    this.fetchData(this.currentPage);
  },
  methods: {
    async fetchData(page) {
      this.isLoading = true;
      try {
        const response = await fetch(
          `https://newsapi.org/v2/top-headlines?country=in&category=${encodeURIComponent(
            this.$route.params.id
          )}&page=${page}&pageSize=10&apiKey=YOUR_API_KEY`
        );
        const data = await response.json();
        //  alert(JSON.stringify(data));
        if (data.status == "ok") {
          this.articles = data.articles;
          this.totalPages = Math.ceil(data.totalResults / 10);
          this.isLoading = false;
        } else {
          this.articles = [];
          this.isLoading = false;
          this.errorMsg = "No articles found";
        }
      } catch (e) {
        this.isLoading = false;
        this.articles = [];
        this.errorMsg =
          "An error occurred, please check your network connection";
      }
    },
    fetchPreviousPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
        this.fetchData(this.currentPage);
        this.scrollToTop();
      }
    },
    fetchNextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
        this.fetchData(this.currentPage);
        this.scrollToTop();
      }
    },
    scrollToTop() {
      window.scrollTo({
        top: 0,
        behavior: "smooth",
      });
    },
  },
  computed: {
    isPreviousDisabled() {
      return this.currentPage === 1;
    },
    isNextDisabled() {
      return this.currentPage === this.totalPages;
    },
  },
};
</script>
