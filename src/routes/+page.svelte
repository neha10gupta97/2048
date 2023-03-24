<script>
	import { onMount } from 'svelte';
	import Counter from './Counter.svelte';

	import './page.scss';

	let size = 4;
	let gameOver = false;
	let score = 0;
	let values = Array.from({ length: size }, () => Array.from({ length: size }));
	values[0][1] = 2;
	values[0][2] = 2;

	onMount(async () => {
		document.addEventListener('keyup', (e) => {
			if (gameOver) {
				return;
			}
			if (e.code === 'ArrowUp') {
				moveUp();
			} else if (e.code === 'ArrowDown') {
				moveDown();
			} else if (e.code === 'ArrowLeft') {
				moveLeft();
			} else if (e.code === 'ArrowRight') {
				moveRight();
			}
		});
		getNewItem();
	});

	const moveUp = () => {
		for (let j = 0; j < size; j++) {
			for (let i = 0; i < size - 1; i++) {
				let currVal = values[i][j];
				let nextVal, index;
				for (let k = i + 1; k < size; k++) {
					if (!nextVal && values[k][j]) {
						nextVal = values[k][j];
						index = k;
					}
				}
				if (!nextVal || !index) {
					continue;
				}
				if (currVal) {
					if (nextVal === currVal) {
						values[i][j] *= 2;
						score += values[i][j];
						values[index][j] = undefined;
					}
				} else {
					values[i][j] = nextVal;
					values[index][j] = undefined;
					i--;
				}
			}
		}
		getNewItem();
	};

	const moveDown = () => {
		for (let j = 0; j < size; j++) {
			for (let i = size - 1; i > 0; i--) {
				let currVal = values[i][j];
				let nextVal, index;
				for (let k = i - 1; k >= 0; k--) {
					if (!nextVal && values[k][j]) {
						nextVal = values[k][j];
						index = k;
					}
				}
				if (!nextVal || index === undefined) {
					continue;
				}
				if (currVal) {
					if (nextVal === currVal) {
						values[i][j] *= 2;
						score += values[i][j];
						values[index][j] = undefined;
					}
				} else {
					values[i][j] = nextVal;
					values[index][j] = undefined;
					i++;
				}
			}
		}
		getNewItem();
	};

	const moveLeft = () => {
		for (let i = 0; i < size; i++) {
			for (let j = 0; j < size - 1; j++) {
				let currVal = values[i][j];
				let nextVal, index;
				for (let k = j + 1; k < size; k++) {
					if (!nextVal && values[i][k]) {
						nextVal = values[i][k];
						index = k;
					}
				}
				if (!nextVal || !index) {
					continue;
				}
				if (currVal) {
					if (nextVal === currVal) {
						values[i][j] *= 2;
						score += values[i][j];
						values[i][index] = undefined;
					}
				} else {
					values[i][j] = nextVal;
					values[i][index] = undefined;
					j--;
				}
			}
		}
		getNewItem();
	};

	const moveRight = () => {
		for (let i = 0; i < size; i++) {
			for (let j = size - 1; j >= 0; j--) {
				let currVal = values[i][j];
				let nextVal, index;
				for (let k = j - 1; k >= 0; k--) {
					if (!nextVal && values[i][k]) {
						nextVal = values[i][k];
						index = k;
					}
				}
				if (!nextVal || index === undefined) {
					continue;
				}
				if (currVal) {
					if (nextVal === currVal) {
						values[i][j] *= 2;
						score += values[i][j];
						values[i][index] = undefined;
					}
				} else {
					values[i][j] = nextVal;
					values[i][index] = undefined;
					j++;
				}
			}
		}
		getNewItem();
	};

	const getEmptySpaces = () => {
		const empty = [];
		for (let i = 0; i < size; i++) {
			for (let j = 0; j < size; j++) {
				if (!values[i][j]) {
					empty.push([i, j]);
				}
			}
		}
		return empty;
	};

	const getNewItem = () => {
		const empty = getEmptySpaces();
		if (!empty.length) {
			gameOver = true;
			return;
		}
		const randomPos = Math.floor(Math.random() * empty.length);
		const [i, j] = empty[randomPos];
		const randomNum = Math.floor(Math.random() * 2) === 0 ? 2 : 4;
		values[i][j] = randomNum;
	};
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>2048</h1>

	<h2>Score: {score}</h2>

	{#if gameOver}
		<h3>Game Over</h3>
	{/if}

	{#each Array.from({ length: size }, (v, i) => i) as row}
		<div class="row">
			{#each Array.from({ length: size }, (v, i) => i) as col}
				<div class="cell">{values[row] ? values[row][col] ?? '' : ''}</div>
			{/each}
		</div>
	{/each}
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
	}
	.row {
		display: flex;
	}
	.cell {
		width: 5rem;
		height: 5rem;
		border: 1px solid #000;
	}
</style>
