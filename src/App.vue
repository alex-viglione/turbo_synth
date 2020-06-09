<template>
	<div id="app">
		<h1>Basic oscillator test</h1>
		<label for="waveType">Choose a wave type:</label>

		<select
			name="waveType"
			id="waveType"
			v-model="wave"
		>
			<option value="sine">sine</option>
			<option value="square">square</option>
			<option value="triangle">triangle</option>
			<option value="sawtooth">sawtooth</option>
		</select>

		<hr>

		<button
			v-for="note in noteValues"
			@click="playSound(note)"
		>Play {{getKeyByValue(noteValues, note)}}</button>

	</div>
</template>

<script>
import { noteValues } from './assets/notevalues.js';

export default {
	name: 'App',
	data() {
		return {
			noteValues,
			selectedNote: null,
			wave: 'sine'
		}
	},
	methods: {
		getKeyByValue(object, value) {
			return Object.keys(object).find(key => object[key] === value);
		}
		,
		playSound(note) {
			const context = new AudioContext();
			const o = context.createOscillator();
			const g = context.createGain();

			o.connect(g);
			o.type = this.wave;
			const frequency = note;

			o.frequency.value = frequency;
			g.connect(context.destination);
			o.start(0);
			setTimeout(() => {
				g.gain.exponentialRampToValueAtTime(
					0.00001, context.currentTime + 1
				);
			}, 50);
		}
	}
}
</script>

<style>
html {
	background-color: #2c3e50;
}
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: white;
	margin-top: 60px;
}
</style>
