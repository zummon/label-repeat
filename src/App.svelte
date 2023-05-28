<script>
	let page;
	let texts = ["Title", "Subtitle"];
	let letterspacings = ["-0.025", "0"];
	let fontsizes = ["16", "18"];
	let color = "#000000";
	let columns = "4";
	let repeat = "24";
	let font = 'None'
	
	let fontList = {
		None: {
			fontFamily: '',
			link: '',
		},
		Kodchasan: {
			fontFamily: "'Kodchasan', sans-serif",
			link: "https://fonts.googleapis.com/css2?family=Kodchasan&display=swap",
		}
	}
</script>

<svelte:head>
	<link href={fontList[font].link} rel="stylesheet">
</svelte:head>

<div
	class="mx-auto flex max-w-3xl flex-wrap items-center justify-center px-2 print:hidden"
>
	<label
		class="m-2 rounded-md border-2 border-dotted border-gray-500 py-1 px-3"
	>
		<span class="text-lg text-gray-400">Color</span>
		<input
			class="border-transparent bg-transparent p-0 text-lg"
			style=""
			type="color"
			bind:value={color}
		/>
	</label>
	<label
		class="m-2 rounded-md border-2 border-dotted border-gray-500 py-1 px-3"
	>
		<span class="text-lg text-gray-400">Columns</span>
		<input
			class="border-transparent bg-transparent p-0 text-lg"
			style="width: 2rem;"
			type="number"
			min="1"
			bind:value={columns}
		/>
	</label>
	<label
		class="m-2 rounded-md border-2 border-dotted border-gray-500 py-1 px-3"
	>
		<span class="text-lg text-gray-400">Repeat</span>
		<input
			class="border-transparent bg-transparent p-0 text-lg"
			style="width: 3rem"
			type="number"
			min="1"
			bind:value={repeat}
		/>
	</label>
	<label
		class="m-2 rounded-md border-2 border-dotted border-gray-500 py-1 px-3"
	>
		<span class="text-lg text-gray-400">Font</span>
		<select
			class="border-transparent bg-transparent p-0 text-lg"
			style=""
			bind:value={font}
		>
			{#each Object.keys(fontList) as value, index (`font-${index}`)}
				<option value={value}>{value}</option>
			{/each}
		</select>
	</label>
</div>

<div
	class="mx-auto mb-2 flex max-w-3xl flex-wrap items-center justify-center px-2 print:hidden"
>
	<div class="m-2 rounded-md border-2 border-dotted border-gray-500 py-1 px-3">
		<span class="text-gray-400">Letter spacing</span>
		{#each letterspacings as letterspacing, i (`letter-spacing-${i}`)}
			<input
				class="block border-transparent bg-transparent p-0 text-lg"
				style="width: 5rem"
				type="number"
				step="0.025"
				bind:value={letterspacing}
			/>
		{/each}
	</div>
	<div class="m-2 rounded-md border-2 border-dotted border-gray-500 py-1 px-3">
		<span class="text-gray-400">Font size</span>
		{#each fontsizes as fontsize, i (`font-size-${i}`)}
			<input
				class="block border-transparent bg-transparent p-0 text-lg"
				style="width: 4rem"
				type="number"
				bind:value={fontsize}
			/>
		{/each}
	</div>
</div>

<div
	class="mx-auto grid resize-x overflow-auto bg-white text-center print:resize-none print:overflow-visible print:mx-0"
	style="grid-template-columns: repeat({columns}, minmax(0, 1fr)); width: 768px; font-family: {fontList[font].fontFamily}"
>
	{#each { length: repeat } as _, i (`repeat-${i}`)}
		<div class="p-2">
			{#each texts as text, id (`text-${i}-${id}`)}
				<div
					class=""
					contenteditable="true"
					style="
						color: {color}; 
						letter-spacing: {letterspacings[id]}em; 
						font-size: {fontsizes[id]}px;
					"
					bind:textContent={text}
				/>
			{/each}
		</div>
	{/each}
</div>

<div class="my-2 mx-auto max-w-lg p-2 text-center print:hidden">
	<button class="hover:text-gray-400 focus:text-gray-400" onclick="print()">
		<svg
			xmlns="http://www.w3.org/2000/svg"
			class="h-10 w-10"
			viewBox="0 0 20 20"
			fill="currentColor"
		>
			<path
				fill-rule="evenodd"
				d="M5 4v3H4a2 2 0 00-2 2v3a2 2 0 002 2h1v2a2 2 0 002 2h6a2 2 0 002-2v-2h1a2 2 0 002-2V9a2 2 0 00-2-2h-1V4a2 2 0 00-2-2H7a2 2 0 00-2 2zm8 0H7v3h6V4zm0 8H7v4h6v-4z"
				clip-rule="evenodd"
			/>
		</svg>
	</button>
</div>

