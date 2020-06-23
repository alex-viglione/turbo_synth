<template>
	<div id="info">
		<p>Hello, thank you for checking out this little app!</p>
		<p>The sound is generated with <a
				href="https://developer.mozilla.org/en-US/docs/Web/API/OscillatorNode"
				target="__blank"
			>OscillatorNode</a> from the basic <a
				href="https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API"
				target="__blank"
			>Web Audio API</a>. Be aware that there is a hardware limitation to which keys can be kept held on your keyboard at the same time, depending on your keyboard.</p>

		<p>Thus, this is not designed to be a full-fledged piano/keyboard, but a fun little toy and learning project.</p>

		<div class="links">

			<a href="https://github.com/alex-viglione/turbo_synth">Github</a>
			<a href="https://alex-viglione.github.io/">My page</a>
		</div>
	</div>
</template>

<style scoped>
#info {
	width: 60%;
	margin: auto;
}

a {
	color: black;
	font-weight: bold;
}

.links {
	display: flex;
	justify-content: space-evenly;
}
</style>