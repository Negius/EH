<script lang="ts">
	import { Spring, Tween } from 'svelte/motion';
	import moment from 'moment';
	let currentDate = moment();

	let year = $state(currentDate.year()),
		month = $state(currentDate.month() + 1),
		day = $state(currentDate.date()),
		hour = $state(currentDate.hour()),
		minute = $state(currentDate.minute()),
		second = $state(currentDate.second());

	const springYear = new Spring(year),
		springMonth = new Spring(month),
		springDay = new Spring(day),
		springHour = new Spring(hour),
		springMinute = new Spring(minute),
		springSecond = new Tween(second);

	let offsetYear = $derived(modulo(springYear.current, 1)),
		offsetMonth = $derived(modulo(springMonth.current, 1)),
		offsetDay = $derived(modulo(springDay.current, 1)),
		offsetHour = $derived(modulo(springHour.current, 1)),
		offsetMinute = $derived(modulo(springMinute.current, 1)),
		offsetSecond = $derived(modulo(springSecond.current, 1));

	function modulo(n: number, m: number) {
		// handle negative numbers
		return ((n % m) + m) % m;
	}

	setInterval(() => {
		currentDate = moment();
		year = currentDate.year();
		month = currentDate.month() + 1;
		day = currentDate.date();
		hour = currentDate.hour();
		minute = currentDate.minute();
		second = currentDate.second();

		springYear.target = year;
		springMonth.target = month;
		springDay.target = day;
		springHour.target = hour;
		springMinute.target = minute;
		springSecond.target = second;
	}, 1000);
</script>

<div class="globe">
	<div class="counter-viewport">
		<div class="counter-digits year" style="transform: translate(0, {100 * offsetYear}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor(springYear.current + 1)}</strong>
			<strong>{Math.floor(springYear.current)}</strong>
		</div>
		<div class="counter-digits">
			<strong>/</strong>
		</div>
		<div class="counter-digits month" style="transform: translate(0, {100 * offsetMonth}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor(springMonth.current + 1)}</strong>
			<strong>{Math.floor(springMonth.current)}</strong>
		</div>
		<div class="counter-digits">
			<strong>/</strong>
		</div>
		<div class="counter-digits day" style="transform: translate(0, {100 * offsetDay}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor(springDay.current + 1)}</strong>
			<strong>{Math.floor(springDay.current)}</strong>
		</div>
		<div class="counter-digits">
			<strong>&nbsp;&nbsp;</strong>
		</div>
		<div class="counter-digits hour" style="transform: translate(0, {100 * offsetHour}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor(springHour.current + 1)}</strong>
			<strong>{Math.floor(springHour.current)}</strong>
		</div>
		<div class="counter-digits">
			<strong>:</strong>
		</div>
		<div class="counter-digits minute" style="transform: translate(0, {100 * offsetMinute}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor(springMinute.current + 1)}</strong>
			<strong>{Math.floor(springMinute.current)}</strong>
		</div>
		<div class="counter-digits">
			<strong>:</strong>
		</div>
		<div class="counter-digits second" style="transform: translate(0, {100 * offsetSecond}%)">
			<strong class="hidden" aria-hidden="true">{Math.floor(springSecond.current + 1)}</strong>
			<strong>{Math.floor(springSecond.current)}</strong>
		</div>
	</div>
</div>

<link rel="stylesheet" href="/resources/css/calendar.css" />
