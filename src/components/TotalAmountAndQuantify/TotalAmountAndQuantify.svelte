<script lang="ts">
	import type { Transaction } from '../../types/types-generic';

	export let data: Array<Transaction>;

	$: totalAmount = data.reduce((currentValue, item) => (currentValue += item.amount), 0);
	$: totalItems = data.length;
</script>

<div class="mt-2 flex gap-4 rounded-2xl bg-white p-4">
	<span class="font-semibold">Total Amount: ${totalAmount.toFixed(2)}</span>
	<span class="font-semibold">Total Items: {totalItems}</span>
</div>
