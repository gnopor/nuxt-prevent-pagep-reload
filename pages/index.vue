<template>
  <div class="container">
    Test to prevent leave page if value is not true. Value : {{ isEditing }}
    <button @click="isEditing = !isEditing">Change value</button>
    <a href="/test"> go to 404 page</a>
  </div>
</template>

<script>
export default {
  data: () => ({
    isEditing: false,
  }),
  beforeMount() {
    window.addEventListener("beforeunload", this.preventNav);
    // this.$once("hook:beforeDestroy", () => {
    //   window.removeEventListener("beforeunload", this.preventNav);
    // });
  },
  beforeDestroy() {
    window.removeEventListener("beforeunload", this.preventNav);
  },
  beforeRouteLeave(to, from, next) {
    if (this.isEditing) {
      if (!window.confirm("Leave without saving?")) {
        return;
      }
      next();
    }
  },
  methods: {
    preventNav(event) {
      if (!this.isEditing) return;
      event.preventDefault();
      event.returnValue = "";
    },
  },
};
</script>

<style>
.container {
  display: flex;
  flex-direction: column;
}
</style>
