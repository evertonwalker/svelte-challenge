<script lang="ts">
	import type { Transaction } from '../../types/types-generic';

	export let data: Array<Transaction> = [];

	let dateFlag = false;

	function sortByDate() {
		dateFlag = !dateFlag;
		if (dateFlag) {
			data = data.sort((itemA, itemB) => {
				const dateA = new Date(itemA.date + 'T00:00:00');
				const dateB = new Date(itemB.date + 'T00:00:00');
				return dateA.getTime() - dateB.getTime();
			});
		} else {
			data = data.sort((itemA, itemB) => {
				const dateA = new Date(itemA.date + 'T00:00:00');
				const dateB = new Date(itemB.date + 'T00:00:00');
				return dateB.getTime() - dateA.getTime();
			});
		}
	}

	let amountFlag = false;

	function sortByAmount() {
		amountFlag = !amountFlag;
		if (amountFlag) {
			data = data.sort((itemA, itemB) => {
				return itemA.amount - itemB.amount;
			});
		} else {
			data = data.sort((itemA, itemB) => {
				return itemB.amount - itemA.amount;
			});
		}
	}

	let descriptionFlag = false;

	function sortByDescription() {
		descriptionFlag = !descriptionFlag;
		if (descriptionFlag) {
			data = data.sort((itemA, itemB) => {
				return itemA.description
					.toLocaleLowerCase()
					.localeCompare(itemB.description.toLocaleLowerCase());
			});
		} else {
			data = data.sort((itemA, itemB) => {
				return itemB.description
					.toLocaleLowerCase()
					.localeCompare(itemA.description.toLocaleLowerCase());
			});
		}
	}
</script>

<div class="h-[350px] overflow-x-auto rounded-lg bg-white shadow-md md:h-[500px]">
	<table class="min-w-full">
		<thead>
			<tr>
				<th
					on:click={sortByDate}
					class="cursor-pointer border-b border-gray-200 bg-gray-100 px-4 py-3 text-left text-xs font-medium tracking-wider text-gray-500 uppercase"
					>Date</th
				>
				<th
					on:click={sortByDescription}
					class="cursor-pointer border-b border-gray-200 bg-gray-100 px-4 py-3 text-left text-xs font-medium tracking-wider text-gray-500 uppercase"
					>Description</th
				>
				<th
					on:click={sortByAmount}
					class="cursor-pointer border-b border-gray-200 bg-gray-100 px-4 py-3 text-left text-xs font-medium tracking-wider text-gray-500 uppercase"
					>Value</th
				>
			</tr>
		</thead>
		<tbody class="divide-y divide-gray-200">
			{#each data as transaction (transaction.id)}
				<tr class="hover:bg-gray-50">
					<td class="px-4 py-3 text-sm whitespace-nowrap text-gray-900"
						>{new Date(transaction.date).toDateString()}</td
					>
					<td class="px-4 py-3 text-sm whitespace-nowrap text-gray-900"
						>{transaction.description}</td
					>
					<td class="px-4 py-3 text-sm whitespace-nowrap text-gray-900"
						>${transaction.amount.toFixed(2)}</td
					>
				</tr>
			{:else}
				<tr>
					<td colspan="3" class="px-4 py-3 text-center text-gray-500"> No values founded </td>
				</tr>
			{/each}
		</tbody>
	</table>
</div>
