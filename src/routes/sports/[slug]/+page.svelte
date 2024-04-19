<script lang="ts">
	import { onMount } from 'svelte';
	import { browser } from '$app/environment';
	import { page } from '$app/stores';
	import sheetsToJson from '@dhanush-npm/google-sheets-to-json';
	import medalImage from '../../../assets/medal.png';

	// let rounds = [
	// 	{
	// 		roundNumber: 1,
	// 		status: 'ended',
	// 		endTime: '12:30PM',
	// 		matches: [
	// 			{
	// 				matchNumber: 1,
	// 				teamA: 'St. Francis College',
	// 				teamB: 'ABC College',
	// 				status: 'ended',
	// 				teamAScore: 20,
	// 				teamBScore: 19
	// 			},
	// 			{
	// 				matchNumber: 2,
	// 				teamA: 'Christ College',
	// 				teamB: 'BNU College',
	// 				status: 'ended',
	// 				teamAScore: 14,
	// 				teamBScore: 17
	// 			},
	// 			{
	// 				matchNumber: 3,
	// 				teamA: 'JNC College',
	// 				teamB: 'BNU College',
	// 				status: 'ended',
	// 				teamAScore: 9,
	// 				teamBScore: 18
	// 			},
	// 			{
	// 				matchNumber: 4,
	// 				teamA: 'NYC College',
	// 				teamB: 'BNU College',
	// 				status: 'ended',
	// 				teamAScore: 19,
	// 				teamBScore: 12
	// 			}
	// 		]
	// 	},
	// 	{
	// 		roundNumber: 2,
	// 		status: 'ongoing',
	// 		matches: [
	// 			{
	// 				matchNumber: 1,
	// 				teamA: 'St. Francis College',
	// 				teamB: 'ABC College',
	// 				status: 'ended',
	// 				teamAScore: 20,
	// 				teamBScore: 19
	// 			},
	// 			{
	// 				matchNumber: 2,
	// 				teamA: 'Christ College',
	// 				teamB: 'BNU College',
	// 				status: 'ended',
	// 				teamAScore: 14,
	// 				teamBScore: 17
	// 			},
	// 			{
	// 				matchNumber: 3,
	// 				teamA: 'JNC College',
	// 				teamB: 'BNU College',
	// 				status: 'ongoing'
	// 			},
	// 			{
	// 				matchNumber: 4,
	// 				teamA: 'NYC College',
	// 				teamB: 'BNU College',
	// 				status: 'ongoing'
	// 			}
	// 		]
	// 	}
	// ];

	function reverseSlug(slug: string) {
		return slug
			.split('-')
			.map((word) => word.charAt(0).toUpperCase() + word.slice(1))
			.join(' ');
	}

	$: loaded = false;

	onMount(() => {
		if (browser) {
			loaded = true;
		}
	});
</script>

<main class="p-4 space-y-4">
	<section>
		<h1 class="text-4xl">{reverseSlug($page.params.slug)}</h1>
	</section>
	<section>
		<table class="w-full text-sm text-left text-gray-500 rounded-md overflow-hidden">
			<thead class="text-gray-700 uppercase bg-gray-50">
				<tr>
					<th scope="col" class="px-4 py-3 text-orange-400"> Name </th>
					<th scope="col" class="px-2 py-3 text-blue-400"> Class </th>
					<th scope="col" class="px-4 py-3 text-green-400"> Points </th>
				</tr>
			</thead>
			{#if loaded}
				<tbody>
					{#await sheetsToJson('AIzaSyC5XzS785yPWZ79swN0mpPW3XVy7Duf5SY', '1sm_rPFo7xDSPuWHn4MG8xMVe8DKZL1KlgSYFhqvDDDk', reverseSlug($page.params.slug).toUpperCase())}
						<p class="bg-white p-2 w-full text-green-400">...waiting</p>
					{:then items}
						{#each items as item, index}
							<tr class="bg-white border-b">
								<th scope="row" class="px-4 py-4 font-medium text-gray-900 whitespace-nowrap">
									<div class="flex items-center space-x-4 pl-2">
										{#if index === 0}
											<div class="quiz-medal">
												<div class="quiz-medal__circle quiz-medal__circle--gold">
													<span> 1 </span>
												</div>
												<div class="quiz-medal__ribbon quiz-medal__ribbon--left"></div>
												<div class="quiz-medal__ribbon quiz-medal__ribbon--right"></div>
											</div>
										{:else if index === 1}
											<div class="quiz-medal">
												<div class="quiz-medal__circle quiz-medal__circle--silver">
													<span> 2 </span>
												</div>
												<div class="quiz-medal__ribbon quiz-medal__ribbon--left"></div>
												<div class="quiz-medal__ribbon quiz-medal__ribbon--right"></div>
											</div>
										{:else if index === 2}
											<div class="quiz-medal">
												<div class="quiz-medal__circle quiz-medal__circle--bronze">
													<span> 3 </span>
												</div>
												<div class="quiz-medal__ribbon quiz-medal__ribbon--left"></div>
												<div class="quiz-medal__ribbon quiz-medal__ribbon--right"></div>
											</div>
										{:else}
											<div class="quiz-medal">
												<div class="quiz-medal__circle quiz-medal__circle--plain">
													<span> 4 </span>
												</div>
												<div class="quiz-medal__ribbon quiz-medal__ribbon--left"></div>
												<div class="quiz-medal__ribbon quiz-medal__ribbon--right"></div>
											</div>
										{/if}

										<div class=" max-w-[100px] text-wrap">{item.name}</div>
									</div>
								</th>
								<td class="px-2 py-4 text-gray-900"> {item.class} </td>
								<td class="px-2 py-4 text-gray-900 text-center"> {item.points} </td>
							</tr>
						{/each}
					{:catch error}
						<p style="color: red">{error.message}</p>
					{/await}
				</tbody>
			{/if}
		</table>
	</section>
	<!-- <section>
		{#each rounds as round}
			{#if round.status === 'ongoing'}
				<div class="bg-white p-2 rounded-2xl divide-y divide-black/10 text-black">
					<div class="pb-1 flex justify-between">
						<div class="text-lg font-semibold px-2">
							<span class="text-green-500">Ongoing</span> <span>-</span>
							<span class="text-amber-500">Round {round.roundNumber}</span>
						</div>
					</div>
					<div class="flex flex-col justify-between items-center">
						<div class="w-full text-sm divide-y">
							{#each round.matches as match}
								{#if match.status === 'ongoing'}
									<div class="flex items-center space-x-2">
										<div class="px-2 font-bold text-blue-500">M{match.matchNumber}</div>
										<div
											class="w-full font-medium text-center mx-auto grid grid-cols-5 justify-evenly py-2"
										>
											<div class="flex justify-center items-center col-span-2">{match.teamA}</div>
											<div class="text-amber-500 flex justify-center items-center">VS</div>
											<div class="flex justify-center items-center col-span-2">{match.teamB}</div>
										</div>
									</div>
								{/if}
							{/each}
						</div>
					</div>
					<div class="px-2 pt-2 text-sm">Match Duration - 1:00PM - 1:40PM</div>
				</div>
			{/if}
		{/each}
	</section>
	<section>
		<h2 class="text-2xl">Rounds Ended</h2>
	</section>
	<section class="space-y-4">
		{#each rounds as round}
			{#if round.status === 'ended'}
				<div class="bg-white/20 rounded-2xl divide-y divide-white/20 p-2">
					<div class="text-lg font-semibold px-2 pb-1 flex justify-between">
						<div class="text-amber-400">Round {round.roundNumber}</div>
						<div class="text-white">{round.endTime}</div>
					</div>
					{#each round.matches as match}
						{#if match.status === 'ended'}
							<div class="py-2 text-sm">
								<div class="px-2 font-bold text-yellow-500">Match {match.matchNumber}</div>
								<div
									class="w-full font-medium text-center mx-auto grid grid-cols-5 justify-evenly p-2"
								>
									<div class="col-span-2">
										<div class="flex flex-col justify-center items-center h-full">
											{#if match.teamAScore > match.teamBScore}
												<img class="w-8 h-8" src={medalImage} alt="" />
											{/if}
											<div>{match.teamA}</div>
											<div>Score: {match.teamAScore}</div>
										</div>
									</div>
									<div class="text-amber-400 flex justify-center items-center col-span-1">VS</div>
									<div class="col-span-2">
										<div class="flex flex-col justify-center items-center h-full">
											{#if match.teamAScore < match.teamBScore}
												<img class="w-8 h-8" src={medalImage} alt="" />
											{/if}
											<div>{match.teamB}</div>
											<div>Score: {match.teamBScore}</div>
										</div>
									</div>
								</div>
							</div>
						{/if}
					{/each}
				</div>
			{/if}
		{/each}
	</section> -->
	<!-- <section>
		<h2 class="text-2xl">Upcoming</h2>
	</section>
	<section class="space-y-4">
		<div class="bg-white/20 p-2 rounded-2xl divide-y divide-white/20">
			<div class="text-lg font-semibold px-2 pb-1 flex justify-between">
				<div class="text-amber-400">Round 4</div>
				<div class="text-white">2:20 PM</div>
			</div>
			<div class="w-full font-semibold text-center mx-auto grid grid-cols-5 justify-evenly pt-2">
				<div class="col-span-2">
					<div class="flex justify-center items-center h-full">{teamA}</div>
				</div>
				<div class="text-amber-400 flex justify-center items-center col-span-1">VS</div>
				<div class="col-span-2">
					<div class="flex justify-center items-center h-full">{teamB}</div>
				</div>
			</div>
		</div>
		<div class="bg-white/20 p-2 rounded-2xl divide-y divide-white/20">
			<div class="text-lg font-semibold px-2 pb-1 flex justify-between">
				<div class="text-amber-400">Round 5</div>
				<div class="text-white">3:20 PM</div>
			</div>
			<div class="w-full font-semibold text-center mx-auto grid grid-cols-5 justify-evenly pt-2">
				<div class="col-span-2">
					<div class="flex justify-center items-center h-full">{teamA}</div>
				</div>
				<div class="text-amber-400 flex justify-center items-center col-span-1">VS</div>
				<div class="col-span-2">
					<div class="flex justify-center items-center h-full">{teamB}</div>
				</div>
			</div>
		</div>
	</section> -->
</main>

<style>
	.quiz-medal {
		margin: 0px;
	}

	.quiz-medal {
		position: relative;
		margin-bottom: 16px;
	}

	.quiz-medal__circle {
		font-family: 'Roboto', sans-serif;
		font-size: 12px;
		font-weight: 500;
		width: 24px;
		height: 24px;
		border-radius: 100%;
		color: white;
		text-align: center;
		line-height: 20px;
		vertical-align: middle;
		position: relative;
		border-width: 0.2em;
		border-style: solid;
		z-index: 1;
		box-shadow:
			inset 0 0 0 #a7b2b8,
			2px 2px 0 rgba(0, 0, 0, 0.08);
		border-color: #edeff1;
		text-shadow: 2px 2px 0 #98a6ad;
		background: linear-gradient(to bottom right, #d1d7da 50%, #c3cbcf 50%);
	}
	.quiz-medal__circle.quiz-medal__circle--gold {
		box-shadow:
			inset 0 0 0 #e6a007,
			2px 2px 0 rgba(0, 0, 0, 0.08);
		border-color: #fadd40;
		text-shadow: 0 0 4px #d99606;
		background: linear-gradient(to bottom right, #f9ad0e 50%, #e89f06 50%);
	}
	.quiz-medal__circle.quiz-medal__circle--silver {
		box-shadow:
			inset 0 0 0 #a7b2b8,
			2px 2px 0 rgba(0, 0, 0, 0.08);
		border-color: #edeff1;
		text-shadow: 0px 0px 4px #98a6ad;
		background: linear-gradient(to bottom right, #d1d7da 50%, #c3cbcf 50%);
	}
	.quiz-medal__circle.quiz-medal__circle--bronze {
		box-shadow:
			inset 0 0 0 #955405,
			2px 2px 0 rgba(0, 0, 0, 0.08);
		border-color: #e3ad23;
		text-shadow: 0 0 4px #7d4604;
		background: linear-gradient(to bottom right, #b16508 50%, #c67007 50%);
	}
	.quiz-medal__circle.quiz-medal__circle--plain {
		box-shadow:
			inset 0 0 0 #055295,
			2px 2px 0 rgba(0, 0, 0, 0.08);
		border-color: #237de3;
		text-shadow: 0 0 4px #04347d;
		background: linear-gradient(to bottom right, #0838b1 50%, #070ac6 50%);
	}
	.quiz-medal__ribbon {
		content: '';
		display: block;
		position: absolute;
		border-style: solid;
		border-width: 4px 6px;
		width: 0;
		height: 12px;
		top: 20px;
	}

	.quiz-medal__ribbon--left {
		border-color: #fc402d #fc402d transparent #fc402d;
		left: 0px;
		transform: rotate(20deg) translateZ(-32px);
	}

	.quiz-medal__ribbon--right {
		left: 14px;
		border-color: #f31903 #f31903 transparent #f31903;
		transform: rotate(-20deg) translateZ(-48px);
	}
</style>
