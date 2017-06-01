<template>
	<div class="minesweeper-home">
		<div class="form-group">
		    <label for="columns">Columns ({{ columns }})</label>
		    <input type="range" min="10" max="20" step="1" class="form-control" id="columns" v-model="columns">
		</div>
		<div class="form-group">
		    <label for="rows">Rows ({{ rows }})</label>
		    <input type="range" min="10" max="20" step="1" class="form-control" id="rows" v-model="rows">
		</div>
		<div class="form-group">
		    <label for="mines">Mines ({{ mines }})</label>
		    <input type="range" min="10" max="20" step="1" class="form-control" id="mines" v-model="mines">
		</div>
		<div class="form-group no-margin">
			<button class="btn btn-info" @click="play" :disabled="waiting">PLAY</button>
		</div>
	</div>
</template>
<script>
	import bus from './utils/bus'

	export default {
		name: 'minesweeper-home',
		data() {
			return {
				mines: 10,
				columns: 10,
				rows: 10,
				waiting: false
			}
		},
		computed: {
			parameters() {
				return {
					mines: this.mines,
					columns:this.columns,
					rows: this.rows
				}
			}
		},
		methods: {
			play() {
				this.waiting = true;
				bus.$emit('play', this.parameters);
			}
		}
	}
</script>
<style>
	.minesweeper-home {
		padding: 10px;
		position: relative;
		overflow: auto;
	}

	.no-margin {
		margin: 0;
	}
</style>