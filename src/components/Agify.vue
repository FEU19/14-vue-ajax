<template>
	<div>
		<section>
			<h2>Agify API demo</h2>
			<p>
				<input type="text" v-model="name" />
				<button @click="getData">Get data</button> <br />
				<button @click="getDataAsync">Get data async</button>
				<button @click="getDataAxios">Get data (axios)</button>
				<button @click="getDataAxiosAsync">Get data (axios, async)</button>
			</p>
			<p v-if="model">
				Out of {{model.count}} people, the average age of {{model.name}} is {{model.age}}.
			</p>
		</section>

		<hr />

		<section>
			<h2>"Slow" API demo</h2>
			<p>
				We simulate a slow API by waiting a short while before sending the real request.
			</p>
			<p>
				Delay: <input type="number" v-model="delay" />
				<button @click="sendSlowRequest"
					:disabled="waitingForResponse">Send request</button>
			</p>
			<p v-if="errorOccurred">
				An error occurred, please try again.
			</p>
			<p v-else-if="model2">
				{{model2.count}} people.
			</p>
			<p v-else>
				No data from API.
			</p>
		</section>
	</div>
</template>

<script>
const baseUrl = 'https://api.agify.io';

export default {
	data: () => ({
		name: '',
		model: null, //{ name, age, count }
		model2: null,
		delay: 2000,
		waitingForResponse: false,
		errorOccurred: false
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
		},

		getDataAxios() {
			this.$http.get(baseUrl, {
				params: { name: this.name }  //  ?name=this.name
			})
			.then(response => {
				console.log('getDataAxios', typeof response.data, response.data);
				this.model = {
					name:  response.data.name,
					age:   response.data.age,
					count: response.data.count
				}
			})
			.catch(error => {
				console.log('Something went wrong', error);
			});
		},
		async getDataAxiosAsync() {
			try {
				let response = await this.$http.get(baseUrl, {
					params: { name: this.name }
				});
				this.model = {
					name:  response.data.name,
					age:   response.data.age,
					count: response.data.count
				}
			} catch(error) {
				console.log('Something went wrong', error);
			}
		},

		sendSlowRequest() {
			this.waitingForResponse = true;
			this.errorOccurred = false;
			setTimeout(this.sendRequestForReal, this.delay);
		},
		async sendRequestForReal() {
			// const url = 'https://cat-fact.herokuapp.com/facts/random';
			// let response = await this.$http.get(url, {
			// 	params: { animal_type: 'cat', amount: '2' }
			// });
			// console.log('Cat facts:', response.data);
			try {
				let response = await this.$http.get(baseUrl, {
					params: { name: this.name }
				});
				console.log('sendRequestForReal', response.data);
				this.model2 = {
					name:  response.data.name,
					age:   response.data.age,
					count: response.data.count
				}
			} catch(error) {
				console.log('Something went wrong', error);
				this.model2 = null;
				this.errorOccurred = true;
			} finally {
				this.waitingForResponse = false;
			}
		}
	}
}
</script>

<style scoped>
button:disabled {
	color: lightgray;
	background-color: white;
}
</style>
