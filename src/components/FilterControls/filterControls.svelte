<script>
	export let minAmountFilter = 0;
	export let maxAmountFilter = 200;
	export let descriptionFilter = '';
	export let startDateFilter = '';
	export let endDateFilter = '';
	export let resetFilters;
</script>

<div
	class="mb-6 flex w-full flex-wrap items-center justify-between rounded-lg bg-gray-50 p-4 shadow-md"
>
	<div class="flex w-full flex-col gap-4">
		<div class="flex flex-wrap items-center justify-between gap-4 md:justify-start">
			<label for="min-amount" class="font-medium text-gray-700">Min Amount:</label>
			<input
				type="number"
				id="min-amount"
				bind:value={minAmountFilter}
				step="0.01"
				class="w-32 rounded-md border border-gray-300 p-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			/>

			<label for="max-amount" class="font-medium text-gray-700">Max Amount:</label>
			<input
				type="number"
				id="max-amount"
				bind:value={maxAmountFilter}
				step="0.01"
				class="w-32 rounded-md border border-gray-300 p-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			/>
		</div>

		<input
			class="w-full rounded-md border border-gray-300 p-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			type="text"
			bind:value={descriptionFilter}
			placeholder="Filter by description"
		/>

		<div
			class="mb-6 flex flex-wrap items-center justify-between gap-4 rounded-lg bg-gray-50 md:justify-start"
		>
			<label for="start-date" class="font-medium text-gray-700">Start Date:</label>
			<input
				type="date"
				id="start-date"
				bind:value={startDateFilter}
				class="rounded-md border border-gray-300 p-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			/>

			<label for="end-date" class="font-medium text-gray-700">End Date:</label>
			<input
				type="date"
				id="end-date"
				bind:value={endDateFilter}
				class="rounded-md border border-gray-300 p-2 focus:ring-2 focus:ring-blue-500 focus:outline-none"
			/>
		</div>
	</div>
	<button
		on:click={resetFilters}
		class="rounded-md bg-blue-600 px-4 py-2 text-white transition-colors duration-200 hover:bg-blue-700"
	>
		Clean filters
	</button>
</div>
