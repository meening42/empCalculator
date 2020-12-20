<template>
  <div>
    <h2>{{ exTitle }}</h2>
    <p>{{ exText }}</p>
  </div>
</template>

<script>
export default {
  name: "ExerciseHeader",
  props: {
    exTitle: String,
    exText: String,
  },
};
</script>

<style scoped></style>
