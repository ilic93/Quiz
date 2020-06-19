<template>
  <div class="header">
    <b-nav tabs>
      <b-nav-item disabled><b>Music Quiz</b></b-nav-item>
      <b-nav-item disabled>Result: {{ correct }}/{{ total }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: {
    correct: Number,
    total: Number
  }
}
</script>
