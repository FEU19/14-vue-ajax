<template>
	<div id="app">
		<h1>AJAX examples</h1>
		<Agify/>	
	</div>
</template>

<script>
import Agify from './components/Agify';

export default {
	name: 'App',
	components: {
		Agify
	}
}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
}
</style>
