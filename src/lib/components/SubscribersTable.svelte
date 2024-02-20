<script lang="ts">
	import { PencilSquare } from 'svelte-heros-v2';
	import Drawer from '$lib/components/Drawer.svelte';
	// Example array of newsletter subscribers
	let subscribers = [
		{
			name: 'Juan Perez',
			email: 'juan.perez@example.com',
			subscriptions: 'Tech Weekly, Health Daily',
			status: 'Activo',
			contribution: '$150',
			updated: '2023-03-15',
			edit: 'Editar'
		},
		{
			name: 'Maria Lopez',
			email: 'maria.lopez@example.com',
			subscriptions: 'Fashion Monthly, Cooking Tips',
			status: 'Pendiente',
			contribution: '$200',
			updated: '2023-03-10',
			edit: 'Editar'
		},
		{
			name: 'Carlos García',
			email: 'carlos.garcia@example.com',
			subscriptions: 'Daily Tech News, Fitness Focus',
			status: 'Cancelado',
			contribution: '$75',
			updated: '2023-02-20',
			edit: 'Editar'
		}
	];
	/**
	 * @type {null}
	 */
	let editingSubscriber: null = null;
	let isDrawerOpen = false;

	/**
	 * @param {{ name: string; email: string; subscriptions: string; status: string; contribution: string; updated: string; edit: string; }} subscriber
	 */
	function openDrawer(subscriber: {
		name: string;
		email: string;
		subscriptions: string;
		status: string;
		contribution: string;
		updated: string;
		edit: string;
	}) {
		editingSubscriber = subscriber;
		isDrawerOpen = true;
	}

	function closeDrawer() {
		isDrawerOpen = false;
	}

	// Function to stop propagation of click events inside the drawer
	/**
	 * @param {{ stopPropagation: () => void; }} event
	 */
	function stopPropagation(event: { stopPropagation: () => void }) {
		event.stopPropagation();
	}

	// Custom transition function for grow and move effect
	function moveFromRight(node: any, { duration }: any) {
		return {
			duration,
			css: (t: number) => `
            transform: translate(${(1 - t) * 10}%, 0);
            opacity: ${t};
        `
		};
	}
</script>

<div class="flex flex-col w-full rounded-lg border">
	<table class="table-auto w-full p-2 bg-slate-50 rounded-lg">
		<thead>
			<tr class="table-header">
				<th class="text-sm text-left">Nombre</th>
				<th class="text-sm text-left">Email</th>
				<th class="text-sm text-left">Suscripciones</th>
				<th class="text-sm text-left">Estado</th>
				<th class="text-sm text-left">Contribución</th>
				<th class="text-sm text-left">Actualizado</th>
				<th class="text-sm text-left">Editar</th>
			</tr>
		</thead>
		<tbody>
			{#each subscribers as subscriber, index}
				<tr class={`table-row hover:bg-slate-100 ${index % 2 === 0 ? 'bg-white' : 'bg-slate-50'}`}>
					<td class="text-sm">{subscriber.name}</td>
					<td class="text-sm">{subscriber.email}</td>
					<td class="text-sm">{subscriber.subscriptions}</td>
					<td class="text-sm">{subscriber.status}</td>
					<td class="text-sm">{subscriber.contribution}</td>
					<td class="text-sm">{subscriber.updated}</td>
					<td class="text-sm">
						<button on:click={() => openDrawer(subscriber)} class="flex place-content-left">
							<PencilSquare size="20" />
						</button>
					</td>
				</tr>
			{/each}
		</tbody>
	</table>
</div>

{#if isDrawerOpen}
	<div class="fixed inset-0 bg-black bg-opacity-20 z-40" on:click={closeDrawer}>
		<!-- <div on:click|stopPropagation transition:moveFromRight={{ duration: 100 }}> -->
		<div>
			<Drawer subscriber={editingSubscriber} onClose={closeDrawer} />
		</div>
	</div>
{/if}

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
