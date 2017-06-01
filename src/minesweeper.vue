<template>
	<div class="minesweeper">
		<minesweeper-home v-if="!squares.length"></minesweeper-home>
		<div v-else>
			<p class="minesweeper-title">{{ title }}</p>
			<minesweeper-grid :squares="squares" :parameters="parameters"></minesweeper-grid>
			<button class="btn btn-info minesweeper-button" @click="goBack">GO BACK</button>
		</div>
	</div>
</template>
<script>
	import minesweeperHome from './minesweeper-home.vue'
	import minesweeperGrid from './minesweeper-grid.vue'
	import bus from './utils/bus'

	export default {
		name: 'minesweeper',
		components: {
			minesweeperHome,
			minesweeperGrid
		},
		data() {
			return {
				id: 0,
				gameOver: false,
				won: false,
				squares: [],
				parameters: {
					columns: 10,
					rows: 10,
					mines: 10
				}
			}
		},
		computed: {
			title() {
				let go = 'GAME #' + this.id;

				if(this.gameOver)
					go += ' - Game over';

				if(this.won)
					go += ' - You won';

				return go;
			}
		},
		methods: {
			goBack() {
				this.squares = [];
			},
			newGame() {
				this.$http.post('/api/games', this.parameters).then((response) => {
		            this.squares = response.data.squares;
		            this.id = response.data.id;
		            this.isGameFinished(response.data.over, response.data.won);
		        });
			},
			isGameFinished(gameOver, won) {
				this.gameOver = gameOver;
				this.won = won;
				if(gameOver || won)
				{
					bus.$emit('finish');
				}
			},
			play(params) {
				this.parameters = params;
				this.newGame();
			},
			load(id) {
				this.$http.get('/api/games/' + id).then((response) => {
		            this.squares = response.data.squares;
		            this.id = response.data.id;
		            this.isGameFinished(response.data.over, response.data.won);
		        });
			},
			reveal(id){
				const url = '/api/games/' + this.id + '/squares/' + id + '/reveal';

				this.$http.patch(url).then((response) => {
					this.squares = response.data.squares;
					this.isGameFinished(response.data.over, response.data.won);
				})
			}
		},
		mounted() {
			bus.$on('reveal', info => this.reveal(info));
			bus.$on('play', info => this.play(info));
			bus.$on('load', info => this.load(info));
		}
	}
</script>
<style>
	body, html {
		box-sizing: border-box;
		height: 100%;
		width: 100%;
		margin: 0;
		padding: 0;
		background-color: #eee;
	}

	.minesweeper {
		position: relative;
		padding: 0 15px;
		font-size: 15px;
		box-shadow: 0px 0px 5px 0px rgba(0,0,0,0.75);
		background-color: #FFF;
		min-width: 300px;
	}

	.minesweeper-title {
		text-align: center;
		margin: 15px;
		font-size: 20px;
		font-weight: bold;
	}

	.minesweeper-button {
		margin: 15px 0;
	}
</style>