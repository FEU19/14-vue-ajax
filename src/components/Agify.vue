<template>
	<section>
		<h2>Agify API demo</h2>
		<p>
			<input type="text" v-model="name" />
			<button @click="getData">Get data</button>
		</p>
		<p v-if="model">
			Out of {{model.count}} people, the average age of {{model.name}} is {{model.age}}.
		</p>
	</section>
</template>

<script>
const baseUrl = 'https://api.agify.io';

export default {
	data: () => ({
		name: '',
		model: null //{ name, age, count }
	}),
	methods: {
		getData() {
			// https://api.agify.io?name=michael
			console.log('getData 1');
			fetch(baseUrl + `?name=${this.name}` // VARNING! osäker kod
				// { method: 'GET', data: { "name": this.name } }
			)
			.then(response => {
				console.log('getData 2', response);
				return response.json();
			})
			.then(data => {
				console.log('getData 3', data);
				this.model = {
					name: data.name,
					age: data.age,
					count: data.count
				}
			})
			console.log('getData END');
		}
	}
}
</script>

<style scoped>

</style>
