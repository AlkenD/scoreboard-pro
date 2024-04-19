<script lang="ts">
	import { GoogleSpreadsheet } from 'google-spreadsheet';
	import { browser } from '$app/environment';
	import { onMount } from 'svelte';
	import { default as sheetsToJson } from '@dhanush-npm/google-sheets-to-json';

	let allSportsTitle: any = [];
	let allSportsData: any = [];
	let sortedData: any = [];

	const eventOrder = [
		'men100m',
		'women100m',
		'men200m',
		'women200m',
		'men400m',
		'women400m',
		'men800m',
		'women800m',
		'men1500m',
		'women1500m',
		'women3000m',
		'men5000m',
		'menLongJump',
		'womenLongJump',
		'menHighJump',
		'womenHighJump',
		'menTripleJump',
		'womenTripleJump',
		'menDiscusThrow',
		'womenDiscusThrow',
		'menJavelinThrow',
		'womenJavelinThrow',
		'menShotput',
		'womenShotput',
		'men100mRelay',
		'women100mRelay'
	];

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
		await sheetsToJson(
			'AIzaSyC5XzS785yPWZ79swN0mpPW3XVy7Duf5SY',
			'1sm_rPFo7xDSPuWHn4MG8xMVe8DKZL1KlgSYFhqvDDDk',
			'SCORES'
		)
			.then(async (data) => {
				allSportsData = data;
				let eventStatus = allSportsData[39];
				sortedData = eventOrder.map((event) => ({
					status: eventStatus[event],
					name: event
				}));
			})
			.catch((error) => {
				console.log(error.message);
			});
		allSportsTitle = sheetTitles;
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
		<h2 class="text-lg text-center font-bold text-yellow-400">Top 10 Class Rankings</h2>
		<table class="w-full text-sm text-left rtl:text-right text-gray-500 rounded-md overflow-hidden">
			<thead class="text-orange-500 uppercase bg-gray-50">
				<tr>
					<th scope="col" class="px-6 py-3"> Class </th>
					<th scope="col" class="px-2 py-3 text-center"> Scores </th>
				</tr>
			</thead>
			{#if loaded}
				<tbody class="w-full">
					{#await sheetsToJson('AIzaSyC5XzS785yPWZ79swN0mpPW3XVy7Duf5SY', '1sm_rPFo7xDSPuWHn4MG8xMVe8DKZL1KlgSYFhqvDDDk', 'SCORES')}
						<p class="bg-white p-2 w-full text-green-400">...waiting</p>
					{:then items}
						{#each items.sort((a, b) => b.total - a.total).slice(0, 10) as item, index}
							<tr class="bg-white border-b space-x-2">
								<th scope="row" class="px-6 py-4 font-medium text-gray-900 whitespace-nowrap">
									<div class="flex items-center">
										{#if item.total === items[0].total}
											<img
												class="h-8 w-8 aspect-square mr-4"
												src="https://cdn-icons-png.flaticon.com/512/5807/5807380.png"
												alt=""
											/>
										{:else}
											<div class="mr-8 pl-2">{index + 1}</div>
										{/if}
										<div>{item.class}</div>
									</div>
								</th>
								<td class="px-6 py-2 text-gray-900 text-center"> {item.total} </td>
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
		<h2 class="text-lg text-center font-bold text-yellow-400">Results</h2>
		<div class="flex space-x-2 text-black">
			<div class="flex items-center space-x-2 bg-white px-2 py-1 rounded-full w-fit">
				<div class="h-2 w-2 bg-green-500 rounded-full"></div>
				<div class="font-bold pr-2">Ongoing</div>
			</div>
			<div class="flex items-center space-x-2 bg-white px-2 py-1 rounded-full w-fit">
				<div class="h-2 w-2 bg-red-500 rounded-full"></div>
				<div class="font-bold pr-2">Ended</div>
			</div>
		</div>
		<table class="w-full text-sm text-left rtl:text-right text-gray-500 rounded-md overflow-hidden">
			<thead class="text-orange-500 uppercase bg-gray-50">
				<tr>
					<th scope="col" class="pl-6 py-3"> Sport </th>
					<th scope="col" class="pr-6 py-3 text-center"> Results </th>
				</tr>
			</thead>
			<tbody>
				{#each allSportsTitle as sport, index}
					<tr class="bg-white border-b">
						<th scope="row" class="pl-6 py-4 font-medium text-gray-900 whitespace-nowrap">
							<div class="flex items-center space-x-2">
								<div>
									{#if sortedData[index].status === 'ONGOING'}
										<div class="h-2 w-2 bg-green-500 rounded-full"></div>
									{:else}
										<div class="h-2 w-2 bg-red-500 rounded-full"></div>
									{/if}
								</div>
								<div>{sport}</div>
							</div>
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
