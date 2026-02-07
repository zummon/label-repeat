<script>
	import { onMount } from "svelte";

	const starterChunk = {
		lines: ["zummon space",'create free web apps'],
		heavies: [600,500],
		spaces: [0,-0.01],
		repeat: 76,
	};

	let chunks = $state([starterChunk]);

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
				prepare[idx] = starterChunk;
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
		if (prepare[0]) {
			chunks = prepare;
		}
	});
</script>

<div class="px-4 pt-4 pb-2 print:hidden text-center font-semibold">
	<button
		class="border-2 border-teal-500 text-teal-500 bg-white p-2 cursor-pointer inline-flex"
		onclick={() => {
			print();
		}}
	>
		<span class="">
			<!-- https://heroicons.com/solid printer -->
			<svg
				xmlns="http://www.w3.org/2000/svg"
				viewBox="0 0 24 24"
				fill="currentColor"
				class="size-6"
			>
				<path
					fill-rule="evenodd"
					d="M7.875 1.5C6.839 1.5 6 2.34 6 3.375v2.99c-.426.053-.851.11-1.274.174-1.454.218-2.476 1.483-2.476 2.917v6.294a3 3 0 0 0 3 3h.27l-.155 1.705A1.875 1.875 0 0 0 7.232 22.5h9.536a1.875 1.875 0 0 0 1.867-2.045l-.155-1.705h.27a3 3 0 0 0 3-3V9.456c0-1.434-1.022-2.7-2.476-2.917A48.716 48.716 0 0 0 18 6.366V3.375c0-1.036-.84-1.875-1.875-1.875h-8.25ZM16.5 6.205v-2.83A.375.375 0 0 0 16.125 3h-8.25a.375.375 0 0 0-.375.375v2.83a49.353 49.353 0 0 1 9 0Zm-.217 8.265c.178.018.317.16.333.337l.526 5.784a.375.375 0 0 1-.374.409H7.232a.375.375 0 0 1-.374-.409l.526-5.784a.373.373 0 0 1 .333-.337 41.741 41.741 0 0 1 8.566 0Zm.967-3.97a.75.75 0 0 1 .75-.75h.008a.75.75 0 0 1 .75.75v.008a.75.75 0 0 1-.75.75H18a.75.75 0 0 1-.75-.75V10.5ZM15 9.75a.75.75 0 0 0-.75.75v.008c0 .414.336.75.75.75h.008a.75.75 0 0 0 .75-.75V10.5a.75.75 0 0 0-.75-.75H15Z"
					clip-rule="evenodd"
				/>
			</svg>
		</span>
		<span>&nbsp; Print </span>
	</button>
	<button
		class="border-2 border-teal-500 text-teal-500 bg-white p-2 cursor-pointer inline-flex"
		onclick={() => {
			shareLink();
		}}
	>
		<span>
			<!-- https://heroicons.com/mini link -->
			<svg
				xmlns="http://www.w3.org/2000/svg"
				viewBox="0 0 20 20"
				fill="currentColor"
				class="size-6"
			>
				<path
					d="M12.232 4.232a2.5 2.5 0 0 1 3.536 3.536l-1.225 1.224a.75.75 0 0 0 1.061 1.06l1.224-1.224a4 4 0 0 0-5.656-5.656l-3 3a4 4 0 0 0 .225 5.865.75.75 0 0 0 .977-1.138 2.5 2.5 0 0 1-.142-3.667l3-3Z"
				/>
				<path
					d="M11.603 7.963a.75.75 0 0 0-.977 1.138 2.5 2.5 0 0 1 .142 3.667l-3 3a2.5 2.5 0 0 1-3.536-3.536l1.225-1.224a.75.75 0 0 0-1.061-1.06l-1.224 1.224a4 4 0 1 0 5.656 5.656l3-3a4 4 0 0 0-.225-5.865Z"
				/>
			</svg>
		</span>
		<span>&nbsp;Duplicate </span>
	</button>
	<button
		class="border-2 border-sky-500 text-sky-500 bg-white p-2 cursor-pointer inline-flex"
		onclick={() => {
			chunks.push(starterChunk);
		}}
	>
		<span>
			{@render plusicon()}
		</span>
		<span class=""> Add chunk </span>
	</button>
</div>

<div class="px-4 pb-4 print:hidden flex flex-wrap justify-center gap-4">
	{#each chunks as chunk, idx}
		<div class="border border-zinc-300">
			<div class="flex justify-between">
				<div class="text-zinc-600 cursor-pointer font-semibold pl-2">
					Chunk {idx + 1}
				</div>
				<div class="">
					<button
						class="text-violet-600 bg-white p-1 cursor-pointer inline-flex"
						onclick={() => {
							chunks.splice(idx, 1);
						}}
					>
						<span class="">
							{@render xicon()}
						</span>
						<span class="font-medium"> Delete chunk </span>
					</button>
				</div>
			</div>
			<div class="text-center">
				<div class="mb-2">
					<label class="">
						<span class="font-medium">Repeat</span>
						<input
							class="p-1 field-sizing-content border-2 border-teal-500"
							type="number"
							placeholder="16"
							min="1"
							max="99"
							bind:value={chunks[idx].repeat}
						/>
						<span class="font-medium">times</span>
					</label>
				</div>
				<div class="">
					<table class="w-fit mx-auto">
						<thead>
							<tr class="font-medium">
								<td class="px-1 border-t-2 border-x-2 border-zinc-300">
									Edit content
								</td>
								<td class="px-1 border-t-2 border-x-2 border-zinc-300">
									Font weight
								</td>
								<td class="px-1 border-t-2 border-x-2 border-zinc-300">
									Letter spacing
								</td>
								<td class=""></td>
							</tr>
						</thead>
						<tbody>
							{#each chunk.lines as value, index}
								<tr>
									<td class="border-2 border-teal-500">
										<label class="">
											<span class=""></span>
											<input
												class="p-1 field-sizing-content"
												type="text"
												placeholder="Lorem, ipsum.."
												bind:value={chunks[idx].lines[index]}
											/>
										</label>
									</td>
									<td class="border-2 border-teal-500">
										<label class="">
											<span class=""></span>
											<input
												class="p-1 field-sizing-content"
												type="number"
												placeholder="400"
												step="100"
												max="900"
												min="100"
												bind:value={chunks[idx].heavies[index]}
											/>
										</label>
									</td>
									<td class="border-2 border-teal-500">
										<label class="">
											<span class=""></span>
											<input
												class="p-1 field-sizing-content"
												type="number"
												placeholder="0.00"
												step="0.01"
												max="0.1"
												min="-0.05"
												bind:value={chunks[idx].spaces[index]}
											/>
										</label>
									</td>
									<td class="border-y-2 border-r-2 border-zinc-300">
										<button
											class="text-violet-500 bg-white cursor-pointer pt-1"
											onclick={() => {
												chunks[idx].lines.splice(index, 1);
											}}
										>
											{@render xicon()}
										</button>
									</td>
								</tr>
							{/each}
							<tr class="">
								<td class="" colspan="99">
									<button
										class="text-sky-500 bg-white cursor-pointer p-1 inline-flex"
										onclick={() => {
											chunks[idx].lines.push(starterChunk.lines[0]);
										}}
									>
										<span>
											{@render plusicon()}
										</span>
										<span class=""> Add line </span>
									</button>
								</td>
							</tr>
						</tbody>
					</table>
				</div>
			</div>
		</div>
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

{#snippet plusicon()}
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
{/snippet}

{#snippet xicon()}
	<!-- https://heroicons.com/micro x-mark -->
	<svg
		xmlns="http://www.w3.org/2000/svg"
		viewBox="0 0 16 16"
		fill="currentColor"
		class="size-6"
	>
		<path
			d="M5.28 4.22a.75.75 0 0 0-1.06 1.06L6.94 8l-2.72 2.72a.75.75 0 1 0 1.06 1.06L8 9.06l2.72 2.72a.75.75 0 1 0 1.06-1.06L9.06 8l2.72-2.72a.75.75 0 0 0-1.06-1.06L8 6.94 5.28 4.22Z"
		/>
	</svg>
{/snippet}
