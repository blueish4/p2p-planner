<script lang="ts">
	import Timeslot from './Timeslot.svelte';
	import times from './times.json';
	const time = times.blocks as any[];
	let tz = 0;
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
	<p>Copyleft AGPLv3, <a href="https://github.com/blueish4/p2p-planner">source available</a></p>
</main>

<style>
	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
	.calendar {
		display: grid;
		grid-template: auto 1fr repeat(22, 1fr) auto / auto;
		column-gap: 10px;
		row-gap: 2px;
		grid-auto-flow: column;
	}
	.header {
		text-align: center;
		height: 0.5fr;
	}
	.tzSelector {
		display: inline-flex;
	}
</style>