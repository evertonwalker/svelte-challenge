<script>
	import data from '$lib/data.json';
	import { onMount } from 'svelte';
	import SmartTable from '../components/SmartTable/SmartTable.svelte';
	import TotalAmountAndQuantify from '../components/TotalAmountAndQuantify/TotalAmountAndQuantify.svelte';
	import FilterControls from '../components/FilterControls/filterControls.svelte';

	let dataToFilter = data;
	let minAmountFilter = 0;
	let maxAmountFilter = 200;
	let descriptionFilter = '';
	let startDateFilter = '';
	let endDateFilter = '';

	let minAmountBasedOnData = 0;
	let maxAmountBasedOnData = 200;
	let minDateBasedOnData = '';
	let maxDateBasedOnData = '';

	function getMinAndMaxValues() {
		dataToFilter.forEach((item) => {
			if (item.amount <= minAmountBasedOnData) {
				minAmountBasedOnData = item.amount;
				minAmountFilter = minAmountBasedOnData;
			}

			if (item.amount >= maxAmountBasedOnData) {
				maxAmountBasedOnData = item.amount;
				maxAmountFilter = maxAmountBasedOnData;
			}
		});
	}

	function prepareDatesToInputs() {
		const { minDate, maxDate } = dataToFilter.reduce(
			(acc, transaction) => {
				const currentDate = new Date(transaction.date);

				if (currentDate < acc.minDate) {
					acc.minDate = currentDate;
				}

				if (currentDate > acc.maxDate) {
					acc.maxDate = currentDate;
				}

				return acc;
			},
			{
				minDate: new Date(dataToFilter[0].date),
				maxDate: new Date(dataToFilter[0].date)
			}
		);

		startDateFilter = `${minDate.getFullYear()}-${String(minDate.getMonth() + 1).padStart(2, '0')}-${String(minDate.getDate()).padStart(2, '0')}`;
		endDateFilter = `${maxDate.getFullYear()}-${String(maxDate.getMonth() + 1).padStart(2, '0')}-${String(maxDate.getDate() + 1).padStart(2, '0')}`;
		minDateBasedOnData = startDateFilter;
		maxDateBasedOnData = endDateFilter;
	}

	function resetFilters() {
		minAmountFilter = minAmountBasedOnData;
		maxAmountFilter = maxAmountBasedOnData;
		startDateFilter = minDateBasedOnData;
		endDateFilter = maxDateBasedOnData;
		descriptionFilter = '';
	}

	$: filteredTransactions = dataToFilter.filter((transaction) => {
		const amount = transaction.amount;
		const matchesAmount = amount >= Number(minAmountFilter) && amount <= Number(maxAmountFilter);

		const matchDescription = transaction.description
			.toLocaleLowerCase()
			.includes(descriptionFilter.toLocaleLowerCase());

		const transactionDate = new Date(transaction.date + 'T00:00:00');

		const startFilterDateObj = startDateFilter ? new Date(startDateFilter + 'T00:00:00') : null;
		const endFilterDateObj = endDateFilter
			? new Date(
					new Date(endDateFilter + 'T00:00:00').setDate(
						new Date(endDateFilter + 'T00:00:00').getDate() + 1
					) - 1
				)
			: null;
		const matchesStartDate =
			!startFilterDateObj || transactionDate.getTime() >= startFilterDateObj.getTime();
		const matchesEndDate =
			!endFilterDateObj || transactionDate.getTime() <= endFilterDateObj.getTime();

		return matchesAmount && matchDescription && matchesStartDate && matchesEndDate;
	});

	onMount(() => {
		prepareDatesToInputs();
		getMinAndMaxValues();
	});
</script>

<div class="flex h-screen justify-center bg-gray-700 p-4">
	<div class="w-full md:w-[992px]">
		<FilterControls
			{resetFilters}
			bind:descriptionFilter
			bind:endDateFilter
			bind:maxAmountFilter
			bind:minAmountFilter
			bind:startDateFilter
		/>
		<SmartTable data={filteredTransactions} />
		<TotalAmountAndQuantify data={filteredTransactions} />
	</div>
</div>
