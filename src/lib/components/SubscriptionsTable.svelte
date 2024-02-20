<script>
	import { getSubscriptions } from '$lib/appwrite';
	import { ComputerDesktop, PencilSquare } from 'svelte-heros-v2';

	let subscriptions = [];	
	export async function fetchSubscriptions() {
		try {
			subscriptions = await getSubscriptions();
			// Do something with subscriptions...
		} catch (error) {
			console.error('An error occurred:', error);
		}
	}
	fetchSubscriptions();
</script>

<div class="flex flex-col w-full rounded-lg border">
	<table class="table-auto w-full p-2 bg-slate-50 rounded-lg">
		<thead>
			<tr class="table-header">
				<th class="text-sm text-left">Suscripción</th>
				<th class="text-sm text-left">Precio</th>
				<th class="text-sm text-left">Frecuencia</th>
				<th class="text-sm text-left">Suscriptores</th>
				<th class="text-sm text-left">Valoración</th>
				<th class="text-sm text-left">Ventas</th>
				<th class="text-sm text-left">Actualizado</th>
				<th class="text-sm text-left">Editar</th>
			</tr>
		</thead>
		<tbody>
			{#each subscriptions as subscription, index}
				<tr class={`table-row hover:bg-slate-100 ${index % 2 === 0 ? 'bg-white' : 'bg-slate-50'}`}>
					<td class="text-sm">{subscription.description}</td>
					<td class="text-sm">{subscription.price}</td>
					<td class="text-sm">{subscription.frequency}</td>
					<td class="text-sm">{subscription.subscribers}</td>
					<td class="text-sm">{subscription.rating}</td>
					<td class="text-sm">{subscription.sales}</td>
					<td class="text-sm">{subscription.lastUpdated}</td>
					<td class="text-sm flex">
						<button class="flex place-content-left">
							<svelte:component this={PencilSquare} size="20" />
						</button>
						<a href="subscription/{subscription.$id}" class="flex place-content-left">
							<svelte:component this={ComputerDesktop} size="20" />
						</a>
					</td>
				</tr>
			{/each}
		</tbody>
	</table>
</div>

<style>
	.table-header th {
		padding: 0.5rem;
		border-bottom: 1px solid #ddd;
	}

	.table-row td {
		padding: 0.5rem;
		border-bottom: 1px solid #eee;
	}
</style>
