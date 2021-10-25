<script lang="ts">
	import Timeslot from './Timeslot.svelte';
	import times from './times.json';
	const time = times.blocks as any[];
	let tz = Math.ceil(-(new Date().getTimezoneOffset()/60));
</script>

<main>
	<div class="tzSelector">
		<input type="range" min="-12" max="12" bind:value="{tz}"/>
		<p>UTC{tz>0?'+':''}{tz}</p>
	</div>
	<div class="calendar">
		<div class="header">
			{(new Date(new Date(time[0].startTime).valueOf() + (tz-1)*60*60*1000)).toLocaleDateString('en-GB', { weekday: 'long' })}
		</div>
		{#each time as block, i}
			<Timeslot name={block.name} tzOffset={tz} isFirst={i===0} startTime={new Date(block.startTime)} endTime={new Date(block.endTime)} background={block.background} />
		{/each}
	</div>
	<p id="copyright">Copyleft AGPLv3, <a href="https://github.com/blueish4/p2p-planner">source available</a></p>
</main>

<style>
	main {

		background: linear-gradient(#905aae, #fe7be1);
	}
	@media (min-width: 640px) {
		main {
			max-width: none;
			height:100vh
		}
	}
	.calendar {
		width: 100%;
		display: block;
	}
	@media (min-width: 640px) {
		.calendar {
			display: grid;
			grid-template: auto repeat(23, 32px) auto / auto;
			column-gap: 10px;
			row-gap: 2px;
			grid-auto-flow: column;
		}
	}

	.header {
		text-align: center;
		height: 0.5fr;
		color: #b1c9ce;
	}
	.tzSelector {
		display: inline-flex;
		color: #b1c9ce;
	}
	#copyright {
		margin-bottom: 0;
	}
</style>