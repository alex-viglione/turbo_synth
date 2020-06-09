<template>
	<div id="app">
		<h1>Testing</h1>
		<button @click="displayNotes">Log notes</button>
	</div>
</template>

<script>
import noteValues from './assets/notevalues.js';

export default {
	name: 'App',
	data() {
		return {
			noteValues
		}
	},
	components: {
	},
	methods: {
		displayNotes() {
			console.log(this.noteValues)
		}
	}
}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
}
</style>
