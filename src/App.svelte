<script>
	import Board from './Board.svelte'
	let points = {player1: 0, player2: 0}
	let	currentPlayer = 1
	let gameOver = false
</script>

<main>
	<h1>Welcome to Revesi !</h1>

	<div class="turn-indicator">
		<p class="turn-message"> Turn: </p>
		{#if currentPlayer === 1}
			<div class="player1-color"/>
		{:else}
			<div class="player2-color"/>
		{/if}
	</div>

	<Board bind:points bind:currentPlayer bind:gameOver boardSize={5} player1Color="tomato" player2Color="steelblue" />

	<div class="scord">
		<div class="player-indicator">
			<div class="player1-color"/>
			<p class="player-score">{points.player1} </p>
		</div>
		
		<div class="player-indicator">
			<div class="player2-color"/>
			<p class="player-score">{points.player2} </p>
		</div>
	</div>

	{#if gameOver}
		<div class="game-over-modal">
			<div class="modal-opacity"/>

			<div class="modal-visible-area">
				<p class="game-over-message"> Game Over! </p>
				<p class="winner-message"> {points.player1 > points.player2 ? 'Player 1 Wins!' : 'Player 2 Wins!'} </p>
				<div class={`player-winner-indicator-${points.player1 > points.player2 ? 1 : 2}`} />
			</div>
		</div>
	{/if}
</main>

<style>
	:global(body) {
		padding: 0;
	}

	main {
		height: 100vh;
		padding: 20px;
		box-sizing: border-box;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	h1 {
		color: #323232;
		margin: 0;
	}

	.player-winner-indicator-1,
	.player-winner-indicator-2 {
		width: 50px;
		height: 50px;
		margin-bottom: 30px;
	}

	.player-winner-indicator-1 {
		background: tomato;
	}
	.player-winner-indicator-2 {
		background: steelblue;
	}
	
	.game-over-modal {
		width: 100%;
		height: 100vh;
		display: flex;
		justify-content: center;
		align-items: center;
		position: absolute;
	}

	.modal-visible-area {
		width: 300px;
		height: 220px;
		background: whitesmoke;
		border-radius: 5px;
		box-shadow: 0 4px 5px gray;
		position: relative;
		z-index: 10;
		display: grid;
		place-items: center;
	}

	.modal-opacity {
		width: 100%;
		height: 100vh;
		position: absolute;
		top: 0;
		left: 0;
		background: rgba(0, 0, 0, .2);
	}

	.game-over-message {

	}

	.turn-indicator {
		margin: 10px 0;
		display: flex;

		justify-content: center;
		align-items: center;
	}

	.turn-message {
		margin: 0 10px 0 0;
	}

	.scord {
		width: 150px;
		margin-top: 40px;
		display: flex;
		flex-direction: column;
		padding: 20px;
		box-sizing: border-box;
		background: whitesmoke;
		border-radius: 5px;
		box-shadow: 0 4px 5px gray;
	}

	.player-indicator {
		display: flex;
		justify-content: flex-start;
		align-items: center;
	}

	.player1-color,
	.player2-color {
		width: 30px;
		height: 30px;
	}

	.player1-color {
		background: tomato;
	}
	.player2-color {
		background: steelblue;
	}

	.player-score {
		padding-left: 20px;
		font-weight: bolder;
	}
</style>