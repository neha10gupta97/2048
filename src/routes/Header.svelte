<script>
	import { page } from '$app/stores';
	import logo from '$lib/images/svelte-logo.svg';
	import github from '$lib/images/github.svg';
	import Counter from './Counter.svelte';
</script>

<header>
	<div class="score-container">Score: <Counter count={0} /></div>
</header>

<style>
	header {
		display: flex;
		justify-content: center;
	}
	.score-container {
		display: flex;
		align-items: center;
	}
</style>
