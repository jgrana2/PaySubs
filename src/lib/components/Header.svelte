<script lang="ts">
	import { Avatar, Dropdown, DropdownHeader, DropdownItem, DropdownDivider } from 'flowbite-svelte';
	import user from '../../store/user';
	import { handleLogout } from '../../store/user';
	import { fade } from 'svelte/transition';

	const avatarStatusColor = 'green';
	const manageSubscriptionsLink = 'manage_subscriptions';
	const manageCardsLink = 'manage_cards';
	let isLoggedIn = false; // Define isLoggedIn with a default false value
	let loggedInUser = {}; // Initialize loggedInUser as an empty object

	user.subscribe((user) => {
		loggedInUser = user;
		// Check if loggedInUser is not null and has properties (not an empty object)
		isLoggedIn = loggedInUser && Object.keys(loggedInUser).length > 0;
	});
</script>

<div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
	<a href="#" class="flex items-center space-x-3 rtl:space-x-reverse">
		<img src="../logo.png" class="h-8" alt="PayGate Logo" />
		<span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white">PayGate</span
		>
	</a>

	{#if isLoggedIn}
		<div transition:fade class="flex h-14 flex-shrink-0 items-center ">
			<!-- User avatar with a green dot indicating availability/status -->
			<Avatar
				id="user-drop"
				class="cursor-pointer"
				alt={`Avatar de ${loggedInUser.name}`}
				dot={{ color: avatarStatusColor }}
			/>

			<!-- Dropdown menu triggered by clicking on the user avatar -->
			<Dropdown triggeredBy="#user-drop">
				<!-- Header section of the dropdown showing user information -->
				<DropdownHeader>
					<span class="block text-sm">{loggedInUser.name}</span>
					<!-- User's name -->
					<span class="block truncate text-sm font-medium">{loggedInUser.email}</span>
					<!-- User's email -->
				</DropdownHeader>

				<!-- Dropdown item for managing subscriptions -->
				<DropdownItem href={manageSubscriptionsLink}>Gestionar suscripciones</DropdownItem>
				<!-- Dropdown item for managing payment methods -->
				<DropdownItem href={manageCardsLink}>Métodos de pago</DropdownItem>

				<!-- Divider line within the dropdown menu -->
				<DropdownDivider />

				<!-- Dropdown item for logging out -->
				<DropdownItem on:click={handleLogout}>Cerrar Sesión</DropdownItem>
			</Dropdown>
		</div>
	{/if}
</div>
