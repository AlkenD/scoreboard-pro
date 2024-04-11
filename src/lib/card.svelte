<script lang="ts">
	import medalImage from '../assets/medal.png';

	export let image: string;
	export let title: string;
	export let info: any = {
		teamA: 'St. Francis College',
		teamAScore: 20,
		teamB: 'XYZ College',
		teamBScore: 15,
		status: 'Ongoing',
		winner: ''
	};

	const getColor = () => {
		if (info.status.toLowerCase() === 'ongoing') {
			return 'bg-green-500';
		} else if (info.status.toLowerCase() === 'ended') {
			return 'bg-red-500';
		} else {
			return 'bg-yellow-500';
		}
	};

	function generateSlug(title: string) {
		return title
			.toLowerCase()
			.replace(/[^a-zA-Z0-9]+/g, '-')
			.replace(/^-+|-+$/g, '');
	}
</script>

<a class="block" href={'/sports/' + generateSlug(title)}>
	<div class="bg-white/20 p-2 rounded-2xl divide-y divide-white/20">
		<div class="pb-1 flex justify-between">
			<div class="text-lg uppercase font-semibold px-2">
				{title}
			</div>
			<div class="px-2 py-1 rounded-full w-fit uppercase text-xs font-bold my-auto {getColor()}">
				{info.status}
			</div>
		</div>
		<div class="flex flex-col justify-between items-center space-y-2">
			<div class="w-24 h-24 aspect-square p-2">
				<img class="object-contain w-full h-full" src={image} alt="" />
			</div>
			<div class="w-full">
				{#if info.winner === ''}
					<div class="px-3 py-0.5 rounded-full bg-white/20 w-fit font-bold text-center mx-auto">
						{info.teamA} <span class="text-amber-400">VS</span>
						{info.teamB}
					</div>
				{:else}
					<div
						class="p-0.5 rounded-full bg-white/20 w-fit font-bold text-center flex space-x-2 justify-center items-center mx-auto pr-3"
					>
						<img class="h-8 w-8 p-1" src={medalImage} alt="" />
						<span class="text-amber-400">{info.winner}</span>
					</div>
				{/if}
			</div>
		</div>
	</div>
</a>
