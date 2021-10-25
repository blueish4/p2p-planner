<script lang="ts">
	export let background: string;
	export let startTime: Date;
	export let endTime: Date;
	export let name: string;
	export let isFirst: boolean;
	export let tzOffset: number;

	let overspill: number;
	const toHour = 60*60*1000;
	let adjustedEndTime: Date;
	let adjustedStartTime: Date;
	let index: number;
	let dayOverlap: boolean;
	let duration: number;

	$: {
		adjustedStartTime = new Date((startTime.valueOf() + (tzOffset-1)*toHour));
		adjustedEndTime = new Date((endTime.valueOf() + (tzOffset-1)*toHour));
		index = isFirst ? adjustedStartTime.getUTCHours()+2 : -1;
		duration = (endTime.valueOf() - startTime.valueOf()) / toHour;
		dayOverlap = adjustedEndTime.getUTCDay() !== adjustedStartTime.getUTCDay();
		if (dayOverlap) {
			console.log(adjustedEndTime)
			overspill = adjustedEndTime.getUTCHours();
			duration = 24 - adjustedStartTime.getUTCHours();
		}
	}
</script>


<div style="background-color: {background};grid-row: {index>0 ? `${index} /` : ''} span {duration};">
	<p>{@html name}
	<br>{adjustedStartTime.getUTCHours()}-{adjustedEndTime.getUTCHours()}</p>
</div>
{#if dayOverlap}
	<div class="header">
		{adjustedEndTime.toLocaleDateString('en-GB', { weekday: 'long' })}
	</div>
	{#if overspill > 0}
	<div style="background-color: {background};grid-row: 2 / span {overspill};">
	</div>
	{/if}
{/if}

<style>
	div {
		text-align: center;
	}
	.header {
		min-height: 0.5fr;
		color: #b1c9ce;
	}
</style>