<template>
	<section>
		<h2>Agify API demo</h2>
		<p>
			<input type="text" v-model="name" />
			<button @click="getData">Get data</button> <br />
			<button @click="getDataAsync">Get data async</button>
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
		// Ett problem med fetch är att det inte finns ett inbyggt sätt att skicka querystring parametrar. Man måste säkra dem manuellt. Därför använder Angular och Vue andra bibliotek. Tyvärr är det så som fetch-standarden är definierad. Exempel på kod som löser problemet finns här: https://github.com/github/fetch/issues/256
		getData() {
			fetch(baseUrl + `?name=${this.name}` // VARNING! osäker kod, lita inte på användaren
			)
			.then(response => {
				return response.json();
			})
			.then(data => {
				this.model = {
					name: data.name,
					age: data.age,
					count: data.count
				}
			})
			.catch(error => {
				console.log('Something went wrong', error);
			})
		},
		async getDataAsync() {
			try {
				let response = await fetch(baseUrl + `?name=${this.name}`);
				let data = await response.json();
				this.model = {
					name: data.name,
					age: data.age,
					count: data.count
				}
			} catch(error) {
				console.log('Something went wrong', error);
			}
		}
	}
}
</script>

<style scoped>

</style>
