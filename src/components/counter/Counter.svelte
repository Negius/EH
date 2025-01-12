<script lang="ts">
	import { Spring } from 'svelte/motion';
	const spring = new Spring(0);
	spring.stiffness = 0.1;
	spring.damping = 0.8;

	let offset = $derived(modulo(spring.current, 1));

	function modulo(n: number, m: number) {
		// handle negative numbers
		return ((n % m) + m) % m;
	}
	
</script>

<div class="counter">
	<button onclick={() => (spring.target -= 1)} aria-label="Decrease the counter by one">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5" />
		</svg>
	</button>

	<div class="counter-viewport">
		<div class="counter-digits" style="transform: translate(0, {100 * offset}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor(spring.current + 1)}</strong>
			<strong>{Math.floor(spring.current)}</strong>
		</div>
	</div>

	<button onclick={() => (spring.target += 1)} aria-label="Increase the counter by one">
		<svg aria-hidden="true" viewBox="0 0 1 1">
			<path d="M0,0.5 L1,0.5 M0.5,0 L0.5,1" />
		</svg>
	</button>
</div>

<link rel="stylesheet" href="/resources/css/counter.css" />
