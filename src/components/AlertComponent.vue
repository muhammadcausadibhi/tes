<template>
  <div>
    <div class="alert alert-danger my-4" role="alert">
      {{ msg }}
    </div>
  </div>
</template>

<script>
export default {
  name: "AlertComponent",
  props: {
    msg: {
      type: String,
      default: "No articles available.",
    },
  },
};
</script>
