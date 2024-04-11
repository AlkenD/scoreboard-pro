<script lang="ts">
	import { page } from '$app/stores';
	import medalImage from '../../../assets/medal.png';

	let winner = '';

	let teamA = 'St. Francis College';

	let teamB = 'ABC College';

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
		<div class="bg-white p-2 rounded-2xl divide-y divide-black/10 text-black">
			<div class="pb-1 flex justify-between">
				<div class="text-lg font-semibold px-2">
					<span class="text-green-500">Ongoing Match</span> <span>-</span>
					<span class="text-amber-500">Round 3</span>
				</div>
			</div>
			<div class="flex flex-col justify-between items-center py-2">
				<div class="w-full text-sm">
					{#if winner === ''}
						<div class="w-full font-semibold text-center mx-auto grid grid-cols-5 justify-evenly">
							<div class="grid grid-cols-3 col-span-2">
								<div class="flex justify-center items-center col-span-2">{teamA}</div>
								<div class="flex justify-center items-center col-span-1">(20)</div>
							</div>
							<div class="text-amber-500 flex justify-center items-center col-span-1">VS</div>
							<div class="grid grid-cols-3 col-span-2">
								<div class="flex justify-center items-center col-span-1">(18)</div>
								<div class="flex justify-center items-center col-span-2">{teamB}</div>
							</div>
						</div>
					{:else}
						<div
							class="p-0.5 rounded-full bg-white/20 w-fit font-bold text-center flex space-x-2 justify-center items-center mx-auto pr-3"
						>
							<img class="h-8 w-8 p-1" src={medalImage} alt="" />
							<span class="text-amber-400">{winner}</span>
						</div>
					{/if}
				</div>
			</div>
			<div class="px-2 pt-2 text-sm">Match Duration - 1:00PM - 1:40PM</div>
		</div>
	</section>
	<section>
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
	</section>
</main>
