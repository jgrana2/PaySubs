<script lang="ts">
	import { Plus } from 'svelte-heros-v2';
	import { Button, Input, Label, Modal, Select, Textarea, Fileupload } from 'flowbite-svelte';
	import { saveSubscription } from '$lib/appwrite';

	// Function to handle the creation of a new subscription
	async function handleSubmit(event: SubmitEvent) {
		event.preventDefault(); // Prevent the form from reloading the page
		const formData = new FormData(event.target);
		const data = {
			description: formData.get('description'),
			frequency: selectedFrequency,
			is_active: false,
			price: formData.get('price')
		};
		try {
			await saveSubscription(data);
			alert('Suscripción creada');
			defaultModal = false;
		} catch (error) {
			console.log('Error creando la suscripción.');
		}
	}
	let defaultModal = false;
	let selectedFrequency: string;
	let opciones = [
		{ value: 'hourly', name: 'Cada hora' },
		{ value: 'daily', name: 'Diaria' },
		{ value: 'weekly', name: 'Semanal' },
		{ value: 'monthly', name: 'Mensual' },
		{ value: 'yearly', name: 'Anual' }
	];
	let filePath: string;
</script>

<div class="absolute bottom-0 right-0 mb-4 mr-4 z-10">
	<Button
		on:click={() => (defaultModal = true)}
		class="w-16 h-16 rounded-full transition-all shadow hover:shadow-lg transform hover:scale-110 flex items-center justify-center"
	>
		<svelte:component this={Plus}></svelte:component>
	</Button>
</div>

<Modal title="Agregar Nueva Suscripción" bind:open={defaultModal} class="min-w-full">
	<form on:submit|preventDefault={handleSubmit}>
		<div class="grid gap-4 mb-4 sm:grid-cols-2">
			<div>
				<Label for="description" class="mb-2">Nombre de la Suscripción</Label>
				<Input
					type="text"
					id="description"
					name="description"
					placeholder="Escriba el nombre de la suscripción"
					required
				/>
			</div>
			<div>
				<Label
					>Frecuencia de Pago
					<Select class="mt-2" items={opciones} bind:value={selectedFrequency} required />
				</Label>
			</div>
			<div>
				<Label for="price" class="mb-2">Price</Label>
				<Input type="text" id="price" placeholder="$29999" name="price" required />
			</div>
			<div>
				<Label class="space-y-2 mb-2">
					<span>Subir Imagen</span>
					<Fileupload bind:filePath />
				</Label>
			</div>
			<div class="sm:col-span-2">
				<Label for="long-description" class="mb-2">Descripción de la Suscripción</Label>
				<Textarea
					id="long-description"
					placeholder="Describa la suscripción"
					rows="4"
					name="long-description"
					required
				/>
			</div>
			<Button type="submit" class="w-52">Agregar suscripción</Button>
		</div>
	</form>
</Modal>
