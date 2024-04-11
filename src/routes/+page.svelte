<script>
	import Card from '$lib/card.svelte';
	import badmintonImage from '../assets/badminton.png';
	import marathonImage from '../assets/finish-line.png';
	import tableTennisImage from '../assets/table-tennis.png';
</script>

<main class="p-4 space-y-4">
	<section>
		<h2 class="text-4xl">Sports</h2>
	</section>
	<section class="space-y-4">
		<Card title="Badminton" image={badmintonImage} />
		<Card title="Marathon 200M" image={marathonImage} />
		<Card title="Marathon 3000M" image={marathonImage} />
		<Card
			info={{
				teamA: 'ABC College',
				teamAScore: 20,
				teamB: 'DSC College',
				teamBScore: 15,
				status: 'Ended',
				winner: 'St. Francis College'
			}}
			title="Table Tennis"
			image={tableTennisImage}
		/>
	</section>
</main>
