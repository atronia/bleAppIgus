<template>
  <img alt="Vue logo" src="./assets/iguslogo.png">
  <HelloWorld msg="Welcome to BWA"/>
	<div>
	<BluetoothScanner />
	</div>
	<div>
		<h3>Ecosystem</h3>
		<p>
			Brought by Smart Plastics, Igus ©
		</p>
	</div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import BluetoothScanner from './components/BluetoothScanner.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
	BluetoothScanner
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
