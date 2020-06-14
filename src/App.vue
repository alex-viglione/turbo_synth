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

		<div class="keyboard">
			<div
				v-for="note in testBoard"
				:key="note.id"
				:class="note.class"
				@click="playSound(note.freq); removeNote(note.freq)"
			>
				<p>{{getKeyByValue(testBoard, note).replace('s', '#')}}</p>
				<p>{{String.fromCharCode(note.keycode)}}</p>
			</div>
		</div>

		<Keypress
			v-for="note in testBoard"
			:key="note.id"
			key-event="keydown"
			:key-code="note.keycode"
			@success="playSound(note.freq)"
		/>
		<Keypress
			v-for="note in testBoard"
			:key="note.id"
			key-event="keyup"
			:key-code="note.keycode"
			@success="removeNote(note.freq)"
		/>
	</div>
</template>

<script>
import { noteValues, testBoard } from './assets/notevalues.js';

export default {
	name: 'App',
	data() {
		return {
			noteValues,
			testBoard,
			selectedNote: null,
			wave: 'sine',
			pressedNotes: [],
		}
	},
	components: {
		Keypress: () => import('vue-keypress')
	},
	methods: {
		getKeyByValue(object, value) {
			return Object.keys(object).find(key => object[key] === value);
		}
		,
		playSound(note) {
			if (this.pressedNotes.includes(note)) {
				return;
			} else {
				this.pressedNotes.push(note);
				const context = new AudioContext();
				const o = context.createOscillator();
				const g = context.createGain();

				o.connect(g);
				g.connect(context.destination);
				o.type = this.wave;
				const frequency = note;
				o.frequency.value = frequency;

				o.start(0);
				o.stop(context.currentTime + 1)
				g.gain.exponentialRampToValueAtTime(
					0.00001, context.currentTime + 2.5
				);
				setTimeout(() => {
					context.close();
				}, 1000)
			}
		},
		removeNote(note) {
			const index = this.pressedNotes.indexOf(note);
			if (index > -1) {
				this.pressedNotes.splice(index, 1);
			}
		}
	},
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

.keyboard {
	width: 90%;
	margin: auto;
	display: flex;
	flex-direction: row;
}

.keyboard div {
	color: black;
	-webkit-touch-callout: none;
	-webkit-user-select: none;
	-khtml-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;
}

.white {
	width: 100px;
	height: 400px;
	background-color: white;
	border: 1px solid black;
}

.black {
	width: 60px;
	height: 240px;
	background-color: black;
	margin: 0 -30px;
	z-index: 2;
}

.black p {
	color: white;
}
</style>
