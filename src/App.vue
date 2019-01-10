<template>
	<div id="app">
		<div class="vue-logo">
			<img alt="Vue logo" src="./assets/logo.png">
			<span>made with <em>VueJS</em></span>
		</div>
		<div class="columns">
			<div class="column is-6 is-offset-3 u-text-center">
				<br>
				<h1>Design & Frontend</h1>
				<h6>Monthly team lunch</h6>
			</div>
		</div>
		<PickerComponent />
	</div>
</template>

<script>
	import PickerComponent from './components/PickerComponent.vue'

	export default {
		name: 'app',
		components: {
			PickerComponent
		},
		mounted() {
			document.getElementById('app').style.minHeight = window.innerHeight + 'px';
		}
	}
</script>

<style lang="scss" scoped>
	h1 {
		font-size: 30px;
		font-weight: bold;
	}
</style>