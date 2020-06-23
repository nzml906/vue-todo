<template>
  <div>
    <label>
      <input type="checkbox" @change="allChecked" />
      Check All
    </label>
  </div>
</template>

<script>
export default {
  name: 'todo-check-all',

  methods: {
    allChecked() {
      this.$store.dispatch('checkAll', event.target.checked);
    }
  }
};
</script>
