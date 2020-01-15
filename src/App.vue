<template>
	<div>
		<div id="app" class="center">
			<h1>Canvas Signer</h1>
			<router-view />
		</div>
	</div>
</template>

<script>
	export default {
		name: 'app',
	}
</script>
<style>
	#app {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #2c3e50;
	}
</style>
