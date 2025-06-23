<script>
	import { onMount } from "svelte";

	let chunks = $state([
		{
			lines: ["title", "subtitle"],
			heavies: [500, 500],
			spaces: [0, 0],
			repeat: 76,
		},
		{
			lines: ["exclusive", "Mr.Tee", "Human"],
			heavies: [500, 500, 500],
			spaces: [0, 0, 0],
			repeat: 4,
		},
	]);

	function shareLink() {
		const searchParams = new URLSearchParams();
		chunks.forEach((chunk, idx) => {
			chunk.lines.forEach((value, index) => {
				searchParams.append(`${idx}-t-${index}`, value);
			});
			chunk.heavies.forEach((value, index) => {
				searchParams.append(`${idx}-w-${index}`, value.toString());
			});
			chunk.spaces.forEach((value, index) => {
				searchParams.append(`${idx}-s-${index}`, value.toString());
			});
			searchParams.append(`${idx}-r`, chunk.repeat.toString());
		});
		window.open(
			"https://codepen.io/zummon/full/qBzPxjj?" + searchParams.toString(),
			"_blank",
		);
		// navigator.clipboard.writeText();
	}

	onMount(() => {
		const searchParams = new URLSearchParams(location.search);
		let prepare = [];
		for (const [shell, value] of searchParams) {
			const slugs = shell.split("-");
			const idx = Number(slugs[0]);
			const key = slugs[1];
			const index = Number(slugs[2]);
			if (!prepare[idx]) {
				prepare[idx] = {
					lines: [],
					heavies: [],
					spaces: [],
					repeat: 4,
				};
			}
			if (key == "t") {
				prepare[idx].lines[index] = value;
			} else if (key == "w") {
				prepare[idx].heavies[index] = Number(value);
			} else if (key == "s") {
				prepare[idx].spaces[index] = Number(value);
			} else if (key == "r") {
				prepare[idx].repeat = Number(value);
			}
		}
		chunks = prepare;
	});
</script>

<div class="p-4 print:hidden text-center">
	<button
		class="border-2 border-teal-500 text-teal-500 bg-white p-2 cursor-pointer"
		onclick={() => {
			print();
		}}
	>
		<!-- https://heroicons.com/solid printer -->
		<svg
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 24 24"
			fill="currentColor"
			class="size-8"
		>
			<path
				fill-rule="evenodd"
				d="M7.875 1.5C6.839 1.5 6 2.34 6 3.375v2.99c-.426.053-.851.11-1.274.174-1.454.218-2.476 1.483-2.476 2.917v6.294a3 3 0 0 0 3 3h.27l-.155 1.705A1.875 1.875 0 0 0 7.232 22.5h9.536a1.875 1.875 0 0 0 1.867-2.045l-.155-1.705h.27a3 3 0 0 0 3-3V9.456c0-1.434-1.022-2.7-2.476-2.917A48.716 48.716 0 0 0 18 6.366V3.375c0-1.036-.84-1.875-1.875-1.875h-8.25ZM16.5 6.205v-2.83A.375.375 0 0 0 16.125 3h-8.25a.375.375 0 0 0-.375.375v2.83a49.353 49.353 0 0 1 9 0Zm-.217 8.265c.178.018.317.16.333.337l.526 5.784a.375.375 0 0 1-.374.409H7.232a.375.375 0 0 1-.374-.409l.526-5.784a.373.373 0 0 1 .333-.337 41.741 41.741 0 0 1 8.566 0Zm.967-3.97a.75.75 0 0 1 .75-.75h.008a.75.75 0 0 1 .75.75v.008a.75.75 0 0 1-.75.75H18a.75.75 0 0 1-.75-.75V10.5ZM15 9.75a.75.75 0 0 0-.75.75v.008c0 .414.336.75.75.75h.008a.75.75 0 0 0 .75-.75V10.5a.75.75 0 0 0-.75-.75H15Z"
				clip-rule="evenodd"
			/>
		</svg>
	</button>
	<button
		class="border-2 border-teal-500 text-teal-500 bg-white p-2 cursor-pointer"
		onclick={() => {
			shareLink();
		}}
	>
		<!-- https://heroicons.com/mini link -->
		<svg
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 20 20"
			fill="currentColor"
			class="size-8"
		>
			<path
				d="M12.232 4.232a2.5 2.5 0 0 1 3.536 3.536l-1.225 1.224a.75.75 0 0 0 1.061 1.06l1.224-1.224a4 4 0 0 0-5.656-5.656l-3 3a4 4 0 0 0 .225 5.865.75.75 0 0 0 .977-1.138 2.5 2.5 0 0 1-.142-3.667l3-3Z"
			/>
			<path
				d="M11.603 7.963a.75.75 0 0 0-.977 1.138 2.5 2.5 0 0 1 .142 3.667l-3 3a2.5 2.5 0 0 1-3.536-3.536l1.225-1.224a.75.75 0 0 0-1.061-1.06l-1.224 1.224a4 4 0 1 0 5.656 5.656l3-3a4 4 0 0 0-.225-5.865Z"
			/>
		</svg>
	</button>
	<button
		class="border-2 border-teal-500 text-teal-500 bg-white p-2 cursor-pointer"
		onclick={() => {
			chunks.push({
				lines: [],
				heavies: [],
				spaces: [],
				repeat: 4,
			});
		}}
	>
		<!-- https://heroicons.com/micro plus -->
		<svg
			xmlns="http://www.w3.org/2000/svg"
			viewBox="0 0 16 16"
			fill="currentColor"
			class="size-6"
		>
			<path
				d="M8.75 3.75a.75.75 0 0 0-1.5 0v3.5h-3.5a.75.75 0 0 0 0 1.5h3.5v3.5a.75.75 0 0 0 1.5 0v-3.5h3.5a.75.75 0 0 0 0-1.5h-3.5v-3.5Z"
			/>
		</svg>
	</button>
	{#each chunks as chunk, idx}
		<details class="">
			<summary class="text-teal-600 cursor-pointer text-lg font-bold">
				{idx + 1}
			</summary>
			<div class="flex flex-wrap justify-center gap-2">
				<div class="">
					<label
						class="inline-block border-2 border-teal-500 font-medium text-teal-600"
					>
						<span class="pl-2">Repeat</span>
						<input
							class="bg-transparent text-center p-1 w-20"
							type="number"
							min="1"
							max="99"
							bind:value={chunks[idx].repeat}
						/>
					</label>
				</div>
				{#each chunk.lines as value, index}
					<div class="flex flex-wrap gap-2">
						<div class="">
							<input
								class="bg-transparent text-center p-1 border-2 border-teal-500"
								type="text"
								placeholder="Content.."
								bind:value={chunks[idx].lines[index]}
							/>
						</div>
						<div class="">
							<input
								class="bg-transparent text-center p-1 w-20 border-2 border-teal-500"
								type="number"
								step="100"
								max="1000"
								min="100"
								bind:value={chunks[idx].heavies[index]}
							/>
						</div>
						<div class="">
							<input
								class="bg-transparent text-center p-1 w-20 border-2 border-teal-500"
								type="number"
								step="0.01"
								bind:value={chunks[idx].spaces[index]}
							/>
						</div>
					</div>
				{/each}
			</div>
		</details>
	{/each}
</div>

<div
	class="flex flex-wrap justify-center gap-y-2 gap-x-3 text-center px-4 print:px-0 mx-auto print:max-w-none"
	style:font-family="'Kodchasan', sans-serif"
	style:max-width="877px"
>
	{#each chunks as chunk, idx}
		{#each Array.from({ length: chunk.repeat }) as i}
			<div class="">
				{#each chunk.lines as value, index}
					{@const heavy = chunks[idx].heavies[index]}
					{@const space = chunks[idx].spaces[index]}
					<div
						class=""
						style:font-weight={isNaN(heavy) ? 400 : heavy}
						style:letter-spacing={`${isNaN(space) ? 0 : space}em`}
					>
						{value}
					</div>
				{/each}
			</div>
		{/each}
	{/each}
</div>
