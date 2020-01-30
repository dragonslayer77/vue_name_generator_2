<script>
export default {
  name: "ChangeTitleInput",
  props: [],
  data() {
    return {
      msg: ""
    };
  },
  methods: {
    changeTitle() {
      this.$emit("changeTitle", this.msg);
      this.clearInput();
    },
    clearInput() {
      this.msg = ""
    },
  }
};
</script>


<template>
  <div class="col-md">
      <input
        class="form-control"
        type="text"
        v-model="msg"
        v-on:keyup.enter="changeTitle"
        placeholder="Change the title here"
      />
  </div>
</template>


<style scoped>
</style>
