<template>
	<div
		v-if="showEdit"
		class="fixed transform -translate-x-1/2 -translate-y-1/2 border top-1/2 left-1/2 bg-indigo-800 z-50 p-4 rounded-lg">
		<p class="text-white p-4">Wprowadź nowy tekst wpisu</p>
		<input class="border-2 border-orange-400 p-4" :placeholder="staryTextWpisu" type="text" v-model="nowyTextWpisu" />
		<div class="grid grid-cols-2 gap-4 items-center p-4">
			<button @click="EditWpis()" class="bg-blue-600 rounded-md text-white p-4 m-auto">OK</button>
			<button @click="hideEditWpis()" class="bg-blue-600 rounded-md text-white p-4 m-auto">Cancel</button>
		</div>
	</div>
	<div>
		<h2 class="text-blue-600">Wpisy na bloga</h2>
		<div class="w-100 flex flex-row-reverse">
			<button @click="pobierzWpisy" class="bg-blue-600 rounded-md text-white p-4">refresh</button>
		</div>
		<div class="grid mx-6 gap-4 my-4">
			<div v-for="(wpis, index) in wpisy" class="drop-shadow-xl bg-stone-300 p-4">
				<p class="p-4">ID: {{ index }}</p>
				<p class="p-4">{{ wpis }}</p>
				<div class="grid grid-cols-2 gap-4 items-center">
					<button @click="deleteWpis(index)" class="bg-blue-600 rounded-md text-white p-4 w-1/4 m-auto">
						Usuń wpis
					</button>
					<button @click="showEditWpis(wpis, index)" class="bg-blue-600 rounded-md text-white p-4 w-1/4 m-auto">
						Edytuj wpis
					</button>
				</div>
			</div>
		</div>
		<div class="flex justify-center flex-col gap-2">
			<input v-model="nowyBlog" type="text" class="border-2 border-blue-600 p-4" />
			<button @click="dodajWpisy" class="bg-blue-600 rounded-md text-white p-4">dodaj</button>
		</div>
	</div>
</template>

<script>
import { dzien2_backend } from "declarations/dzien2_backend/index"
export default {
	data() {
		return {
			wpisy: [],
			nowyBlog: "",
			showEdit: false,
			editIndex: 0,
			staryTextWpisu: "",
		}
	},
	methods: {
		async dodajWpisy() {
			await dzien2_backend.dodaj_wpis(this.nowyBlog)
			await this.pobierzWpisy()
		},
		async deleteWpis(index) {
			await dzien2_backend.usun_wpis(index)
			await this.pobierzWpisy()
		},
		async EditWpis() {
			await dzien2_backend.edytuj_wpis(this.editIndex, this.nowyTextWpisu)
			this.nowyTextWpisu = ""
			await this.hideEditWpis()
			await this.pobierzWpisy()
		},
		async pobierzWpisy() {
			this.wpisy = await dzien2_backend.odczytaj_wpisy()
		},
		async showEditWpis(wpis, index) {
			this.staryTextWpisu = wpis
			this.editIndex = index
			this.showEdit = true
		},
		async hideEditWpis() {
			this.showEdit = false
		},
	},
	async mounted() {
		this.pobierzWpisy()
	},
}
</script>
