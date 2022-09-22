<template>
  <header>
    <h1>{{ title }}</h1>
		<Button :text="showForm ? 'Close' : 'Add Task'" :color="showForm ? 'red' : 'green'" @btn-click="$emit('toggle-add-task')"/>
  </header>
</template>

<script>
	import Button from "./Button.vue"

  export default {
		name: "Header",
		props: ['title', 'showForm'],
		components: {
			Button
		}
  }
</script>

<style scoped>
  header {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 20px;
  }
</style>