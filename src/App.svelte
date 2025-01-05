<script>
	import { onMount } from 'svelte'

	let repeat = $state(76)
	let ts = $state(["title", "subtitle"])
	let ws = $state([500, 500])
	let lss = $state([0, 0])

	onMount(() => {
		const searchParams = new URLSearchParams(location.search);
		let prepare = {
			ts: searchParams.get("t"),
			ws: searchParams.get("w"),
			lss: searchParams.get("ls")
		};
		if (prepare.ts) {
			ts = prepare.ts.split(",");
		}
		if (prepare.ws) {
			ws = prepare.ws.split(",");
		}
		if (prepare.lss) {
			lss = prepare.lss.split(",");
		}
	})

</script>

<div class="flex flex-wrap justify-center gap-2 p-4 print:hidden">
	<div class="">
		<label class="inline-block border-2 border-teal-500 font-medium text-teal-600">
			<span class="pl-2">Count</span>
			<input class="bg-transparent text-center p-1 w-20" type="number" bind:value={repeat}>
		</label>
	</div>
	{#each ts as part, index}
		<div class="flex flex-wrap gap-2">
			<div class="">
				<input class="bg-transparent text-center p-1 border-2 border-teal-500" type="text" bind:value={ts[index]}>
			</div>
			<div class="">
				<input class="bg-transparent text-center p-1 w-20 border-2 border-teal-500" type="number" step="100" max="1000" min="100" bind:value={ws[index]}>
			</div>
			<div class="">
				<input class="bg-transparent text-center p-1 w-20 border-2 border-teal-500" type="number" step="0.01" bind:value={lss[index]}>
			</div>
		</div>
	{/each}
</div>

<div class="flex flex-wrap justify-center gap-y-2 gap-x-3 text-center px-4 print:px-0 mx-auto max-w-[877px] print:max-w-none" style="font-family: 'Kodchasan', sans-serif">
	{#each Array.from({ length: repeat }) as i}
		<div class="">
			{#each ts as part, index}
				<div class="" style="font-weight: {isNaN(ws[index]) ? 400 : ws[index] }; letter-spacing: {isNaN(lss[index]) ? 0 : lss[index] }em;" >{ts[index]}</div>
			{/each}
		</div>
	{/each}
</div>


