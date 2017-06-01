<template>
	<div class="minesweeper-grid" :style="sizing">
		<minesweeper-square v-for="s in squares" :key="s.id" :s="s"></minesweeper-square>
	</div>
</template>
<script>
	import minesweeperSquare from './minesweeper-square.vue'

	export default {
		name: 'minesweeper-grid',
		components: {
			minesweeperSquare
		},
		props: ['squares', 'parameters'],
		computed: {
			sizing() {
				return {
					'height': this.parameters.rows * 30 + 'px',
					'width': this.parameters.columns * 30 + 'px'
				}
			}
		},
	}
</script>
<style>
	.minesweeper-grid {
		position: relative;
	}
</style>