<template>
	<div id="app">
		<div class="col">
			<div class="param">
				<input
					type="radio"
					id="all"
					name="all"
					value="all"
					v-model="sortParams"
				/>
				<label for="contactChoice1">Все</label>
			</div>
			<div class="param">
				<input
					type="radio"
					id="less_then_zero"
					name="less_then_zero"
					value="lessThenZero"
					v-model="sortParams"
				/>
				<label for="contactChoice2">меньше 0 </label>
			</div>
			<div class="param">
				<input
					type="radio"
					id="real_date"
					name="real_date"
					value="realDate"
					v-model="sortParams"
				/>
				<label for="contactChoice3">больше или равно 0 </label>
			</div>
			<div class="param">
				<input
					type="checkbox"
					id="sorted_arr"
					name="sorted_arr"
					value="sortedArr"
					v-model="sortBydate"
				/>
				<label for="date">сортировка по дате </label>
			</div>
		</div>
		<div class="col">
			<div
				class="date"
				v-for="date in dateArr"
				:key="date.id"
				v-show="hide(date)"
			>
				<input
					type="checkbox"
					:id="'date_checkbox' + date.id - 1"
					:value="date.id"
					v-model="checkedDates"
				/>
				<label :for="'date_checkbox' + date.id - 1"
					>ID: {{ date.id }}, Date: {{ date.amount }}</label
				>
			</div>
		</div>
		<div class="col">
			<div
				class="date"
				v-for="date in memoryArr"
				:key="date.id"
			>
				ID: {{ date.id }}, Date: {{ date.amount }}
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: "App",
	components: {},
	data() {
		return {
			dateArr: [],
			checkedDates: [],
			sortParams: [],
			sortBydate: false,
		};
	},

	computed: {
		memoryArr() {
			const memoryArr = this.dateArr.filter((e) => {
				return this.checkedDates.indexOf(e.id) !== -1
			});
			return memoryArr;
		},
	},
	watch: {
		sortBydate(bool) {
			if (bool) {
				this.dateArr.sort((a, b) => b.amount - a.amount);
			} else {
				this.dateArr.sort((a, b) => a.id - b.id);
			}
		},
	},
	methods: {
		getRandomArbitrary(min, max) {
			return Math.floor(Math.random() * (max - min) + min);
		},
		hide(date) {
			if (this.sortParams === "lessThenZero" && date.amount < 0) {
				return true;
			} else if (this.sortParams === "realDate" && date.amount >= 0) {
				return true;
			} else if (
				this.sortParams.length < 1 ||
				this.sortParams === "all"
			) {
				return true;
			}
			return false;
		},
	},
	mounted() {
		for (let i = 0; i < 100; i += 1) {
			this.dateArr.push({
				id: i + 1,
				amount: this.getRandomArbitrary(-1000, 1000),
			});
		}
	},
};
</script>

<style>
#app {
	display: flex;
}
.col {
	width: 33%;
	display: flex;
	flex-wrap: wrap;
	align-content: baseline;
}
.col .param,
.col .date {
	width: 100%;
}
</style>
