<script lang="ts">
	import { GoogleSpreadsheet } from 'google-spreadsheet';
	import { browser } from '$app/environment';
	import { onMount } from 'svelte';
	import sheetsToJson from '@dhanush-npm/google-sheets-to-json';

	let allSports: any = [];

	$: loaded = false;

	onMount(async () => {
		if (browser) {
			loaded = true;
		}
		const doc = new GoogleSpreadsheet('1sm_rPFo7xDSPuWHn4MG8xMVe8DKZL1KlgSYFhqvDDDk', {
			apiKey: 'AIzaSyC5XzS785yPWZ79swN0mpPW3XVy7Duf5SY'
		});
		await doc.loadInfo();
		const sheetTitles = [];
		for (let i = 1; i < doc.sheetsByIndex.length; i++) {
			sheetTitles.push(doc.sheetsByIndex[i].title);
		}
		allSports = sheetTitles;
	});

	function generateSlug(title: string) {
		return title
			.toLowerCase()
			.replace(/[^a-zA-Z0-9]+/g, '-')
			.replace(/^-+|-+$/g, '');
	}
</script>

<main class="p-4 space-y-4">
	<section>
		<h2 class="text-4xl text-center">3rd Annual Athelitic Meet</h2>
	</section>
	<section class="space-y-2">
		<h2 class="text-lg text-center font-bold text-yellow-400">Top 5 Class Rankings</h2>
		<table class="w-full text-sm text-left rtl:text-right text-gray-500 rounded-md overflow-hidden">
			<thead class="text-orange-500 uppercase bg-gray-50">
				<tr>
					<th scope="col" class="px-6 py-3"> Class </th>
					<th scope="col" class="px-6 py-3 text-center"> Scores </th>
				</tr>
			</thead>
			{#if loaded}
				<!-- <tbody>
				{#each allSports.slice(0, 5) as sport}
					<tr class="bg-white border-b">
						<th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">
							{sport}
						</th>
						<td class="px-6 py-4 text-blue-500 text-center"> <a href="/">View</a> </td>
					</tr>
				{/each}
			</tbody> -->
				<tbody>
					{#await sheetsToJson('AIzaSyC5XzS785yPWZ79swN0mpPW3XVy7Duf5SY', '1sm_rPFo7xDSPuWHn4MG8xMVe8DKZL1KlgSYFhqvDDDk', 'SCORES')}
						<p>...waiting</p>
					{:then items}
						{#each items.sort((a, b) => b.total - a.total).slice(0, 5) as item}
							<tr class="bg-white border-b">
								<th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">
									{item.class}
								</th>
								<td class="px-6 py-4 text-gray-900 text-center"> {item.total} </td>
							</tr>
						{/each}
					{:catch error}
						<p style="color: red">{error.message}</p>
					{/await}
				</tbody>
			{/if}
		</table>
	</section>
	<section class="space-y-4">
		<table class="w-full text-sm text-left rtl:text-right text-gray-500 rounded-md overflow-hidden">
			<thead class="text-orange-500 uppercase bg-gray-50">
				<tr>
					<th scope="col" class="pl-6 py-3"> Sport </th>
					<th scope="col" class="pr-6 py-3 text-center"> Scores </th>
				</tr>
			</thead>
			<tbody>
				{#each allSports as sport}
					<tr class="bg-white border-b">
						<th scope="row" class="pl-6 py-4 font-medium text-gray-900 whitespace-nowrap">
							{sport}
						</th>
						<td class="pr-6 py-4 text-blue-500 text-center">
							<a href={'/sports/' + generateSlug(sport)}>View</a>
						</td>
					</tr>
				{/each}
			</tbody>
		</table>
	</section>
</main>
