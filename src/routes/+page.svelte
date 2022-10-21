<script lang="ts">
	import { onMount } from 'svelte';

	let weather: any;

	onMount(async () => {
		const response = await fetch(
			'https://api.open-meteo.com/v1/forecast?latitude=59.9138&longitude=10.7387&hourly=temperature_2m,relativehumidity_2m,apparent_temperature,precipitation'
		);
		const body = await response.json();

		weather = body;
	});
</script>

<h1>Væpp</h1>

{#if weather}
	<table>
		<tbody>
			<tr>
				<th>Time</th>
				{#each weather.hourly.time as time}
					<td
						>{new Date(time).toLocaleString('nb-NO', {
                            day: "numeric",
                            weekday: "long",
                            month: "short",
                            hour: "2-digit",
                        })}</td
					>
				{/each}
			</tr>
            <tr>
                <th>Temperature</th>
                {#each weather.hourly.temperature_2m as temperature_2m}
                    <td>{temperature_2m} {weather.hourly_units.temperature_2m}</td>
                {/each}
            </tr>
            <tr>
                <th>ApparentTemperature</th>
                {#each weather.hourly.apparent_temperature as apparent_temperature}
                    <td>{apparent_temperature} {weather.hourly_units.apparent_temperature}</td>
                {/each}
            </tr>
			<tr>
				<th>Relative Humidity</th>
				{#each weather.hourly.relativehumidity_2m as relativehumidity_2m}
					<td>{relativehumidity_2m} {weather.hourly_units.relativehumidity_2m}</td>
				{/each}
			</tr>
            <tr>
				<th>Precipitation</th>
				{#each weather.hourly.precipitation as precipitation}
					<td>{precipitation} {weather.hourly_units.precipitation}</td>
				{/each}
			</tr>
		</tbody>
	</table>
{/if}
<pre>{JSON.stringify(weather, null, 4)}</pre>

<style>
    th, td {
        padding: 0.5rem;
        white-space: nowrap;

    }

</style>