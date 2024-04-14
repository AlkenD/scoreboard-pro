<script lang="ts">
	import { page } from '$app/stores';
	import medalImage from '../../../assets/medal.png';

	let rounds = [
		{
			roundNumber: 1,
			status: 'ended',
			endTime: '12:30PM',
			matches: [
				{
					matchNumber: 1,
					teamA: 'St. Francis College',
					teamB: 'ABC College',
					status: 'ended',
					teamAScore: 20,
					teamBScore: 19
				},
				{
					matchNumber: 2,
					teamA: 'Christ College',
					teamB: 'BNU College',
					status: 'ended',
					teamAScore: 14,
					teamBScore: 17
				},
				{
					matchNumber: 3,
					teamA: 'JNC College',
					teamB: 'BNU College',
					status: 'ended',
					teamAScore: 9,
					teamBScore: 18
				},
				{
					matchNumber: 4,
					teamA: 'NYC College',
					teamB: 'BNU College',
					status: 'ended',
					teamAScore: 19,
					teamBScore: 12
				}
			]
		},
		{
			roundNumber: 2,
			status: 'ongoing',
			matches: [
				{
					matchNumber: 1,
					teamA: 'St. Francis College',
					teamB: 'ABC College',
					status: 'ended',
					teamAScore: 20,
					teamBScore: 19
				},
				{
					matchNumber: 2,
					teamA: 'Christ College',
					teamB: 'BNU College',
					status: 'ended',
					teamAScore: 14,
					teamBScore: 17
				},
				{
					matchNumber: 3,
					teamA: 'JNC College',
					teamB: 'BNU College',
					status: 'ongoing'
				},
				{
					matchNumber: 4,
					teamA: 'NYC College',
					teamB: 'BNU College',
					status: 'ongoing'
				}
			]
		}
	];

	function reverseSlug(slug: string) {
		return slug
			.split('-')
			.map((word) => word.charAt(0).toUpperCase() + word.slice(1))
			.join(' ');
	}
</script>

<main class="p-4 space-y-4">
	<section>
		<h1 class="text-4xl">{reverseSlug($page.params.slug)}</h1>
	</section>
	<section>
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
	</section>
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
