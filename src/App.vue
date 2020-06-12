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

		<!-- <button
			v-for="note in noteValues"
			@click="playSound(note)"
		>Play {{getKeyByValue(noteValues, note)}}</button> -->

		<div class="keyboard">
			<div class="white_key">
				<p>Q</p>
				<p>C4</p>
			</div>
			<div class="black_key">
				<p>2</p>
				<p>C#4</p>
			</div>
			<div class="white_key">
				<p>W</p>
				<p>D4</p>
			</div>
			<div class="black_key">
				<p>3</p>
				<p>D#4</p>
			</div>
			<div class="white_key">
				<p>E</p>
				<p>E4</p>
			</div>
			<div class="white_key">
				<p>R</p>
				<p>F</p>
			</div>
			<div class="black_key">
				<p>5</p>
				<p>F#4</p>
			</div>
			<div class="white_key">
				<p>T</p>
				<p>G4</p>
			</div>
			<div class="black_key">
				<p>6</p>
				<p>G#4</p>
			</div>
			<div class="white_key"></div>
			<div class="black_key"></div>
			<div class="white_key"></div>
			<div class="white_key"></div>
		</div>

		<Keypress
			key-event="keydown"
			:key-code="81"
			@success="playSound(noteValues.C4)"
		/>

		<Keypress
			key-event="keyup"
			:key-code="81"
			@success="removeNote(noteValues.C4)"
		/>

		<Keypress
			key-event="keydown"
			:key-code="50"
			@success="playSound(noteValues.Cs4)"
		/>
		<Keypress
			key-event="keydown"
			:key-code="87"
			@success="playSound(noteValues.D4)"
		/>
		<Keypress
			key-event="keydown"
			:key-code="51"
			@success="playSound(noteValues.Ds4)"
		/>
		<Keypress
			key-event="keydown"
			:key-code="69"
			@success="playSound(noteValues.E4)"
		/>
		<Keypress
			key-event="keydown"
			:key-code="82"
			@success="playSound(noteValues.F4)"
		/>
		<Keypress
			key-event="keydown"
			:key-code="53"
			@success="playSound(noteValues.Fs4)"
		/>
		<Keypress
			key-event="keydown"
			:key-code="84"
			@success="playSound(noteValues.G4)"
		/>
		<Keypress
			key-event="keydown"
			:key-code="54"
			@success="playSound(noteValues.Gs4)"
		/>
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
			wave: 'sine',
			pressedNotes: []
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
}

.white_key {
	width: 100px;
	height: 400px;
	background-color: white;
	border: 1px solid black;
}

.black_key {
	width: 60px;
	height: 240px;
	background-color: black;
	margin: 0 -30px;
	z-index: 2;
}

.black_key p {
	color: white;
}
</style>
