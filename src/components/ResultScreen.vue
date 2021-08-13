<template>
  <div class="screen">
    <div class="screen__inner">
      <h1>✨ Congratulations ✨</h1>
      <h3>{{ Math.round(timer / 920) }} seconds</h3>
      <button @click="onStartAgain">Start Again</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    timer: {
      type: String,
      required: true,
    },
  },
  methods: {
    onStartAgain() {
      this.$emit("onStartAgain");
    },
  },
};
</script>

<style lang="css">
.screen {
  background-color: var(--dark);
  color: var(--light);
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
}
.screen__inner {
  width: 50%;
  height: 50%;
  text-align: center;
  font-size: 2rem;
}
.screen__inner button {
  font: var(--font-size);
  color: var(--light);
  width: 150px;
  height: 100px;
  background: none;
  box-shadow: none;
  border: 1px solid;
  border-radius: 1rem;
  cursor: pointer;
  margin: 1rem;
  transition: background 0.3s ease-in-out;
}

.screen__inner button:hover {
    background-color: var(--light);
    color: var(--dark);
}
</style>
