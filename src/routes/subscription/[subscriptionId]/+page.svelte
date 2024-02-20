<script lang="ts">
	import { onMount } from 'svelte';
	import Header from '$lib/components/Header.svelte';
	import { Rating, Badge } from 'flowbite-svelte';
	import user from '../../../store/user';

	/** @type {import('./$types').PageData} */
	export let data; // Subscription ID from url parameters

	let subscription = {}; // Create a writable store for the subscription data
	let loggedInUser = {}; // Initialize loggedInUser as an empty object
	let isLoggedIn = false;

	user.subscribe((user) => {
		loggedInUser = user;
		// Check if loggedInUser is not null and has properties (not an empty object)
		isLoggedIn = loggedInUser && Object.keys(loggedInUser).length > 0;
	});

	onMount(async () => {
		const response = await fetch(`http://127.0.0.1:5000/subscription/${data.id}`);
		if (response.ok) {
			const jsonData = await response.json();
			subscription = jsonData;
		}
	});

	async function pay_subscription(e) {
		e.preventDefault();
		if (isLoggedIn) {
			try {
				const hasCards = await userHasCards(loggedInUser.$id);
				if (hasCards.hasCards == 'false') {
					// Redireccionar para agregar método de pago
					window.location.href = `../../payment_method/${data.id}`;
				} else {
					try {
						const response = await fetch('http://127.0.0.1:5000/activate-subscription', {
							method: 'POST',
							headers: {
								'Content-Type': 'application/json'
							},
							body: JSON.stringify({
								subscription_id: subscription.id,
								user_email: loggedInUser.email
							})
						});

						if (!response.ok) {
							if (subscription.is_active) {
								alert('Error al suscribirse. La suscripción se encuentra activa.');
							} else {
								alert('Error al suscribirse. Falló el pago.');
							}
							throw new Error(`HTTP error! status: ${response.status}`);
						}
						
						// At this point, the payment is successful
						const data = await response.json();
						console.log(data);
						window.location.href = '../success';
					} catch (error) {
						console.error('There was an error activating the subscription:', error);
					}
				}
			} catch (error) {
				console.error('There was an error retrieving your cards:', error);
				alert('There was an error processing your payment method. Please try again later.');
				return; // Early return on error
			}
		} else {
			alert('Por favor inicie sesión para suscribirse.');
			window.location.href = `../login/${data.id}`;
		}
	}

	async function userHasCards(userId) {
		// Placeholder for actual implementation to get user cards
		// This will vary depending on your backend API and database
		// Replace with the actual API endpoint and logic to retrieve user's cards
		const response = await fetch(`http://127.0.0.1:5000/cards?user_id=${userId}`, {
			method: 'GET',
			headers: {
				'Content-Type': 'application/json'
			}
		});

		if (!response.ok) {
			throw new Error(`HTTP error while fetching cards! status: ${response.status}`);
		}

		return await response.json();
	}
</script>

<Header></Header>

<div class="flex flex-col items-center justify-center px-4">
	<a href="#" class="group relative block overflow-hidden">
		<button
			class="absolute end-4 top-4 z-10 rounded-full bg-white p-1.5 text-gray-900 transition hover:text-gray-900/75"
		>
			<span class="sr-only">Wishlist</span>

			<svg
				xmlns="http://www.w3.org/2000/svg"
				fill="none"
				viewBox="0 0 24 24"
				stroke-width="1.5"
				stroke="currentColor"
				class="h-4 w-4"
			>
				<path
					stroke-linecap="round"
					stroke-linejoin="round"
					d="M21 8.25c0-2.485-2.099-4.5-4.688-4.5-1.935 0-3.597 1.126-4.312 2.733-.715-1.607-2.377-2.733-4.313-2.733C5.1 3.75 3 5.765 3 8.25c0 7.22 9 12 9 12s9-4.78 9-12z"
				/>
			</svg>
		</button>

		<img
			src="https://images.unsplash.com/photo-1599481238640-4c1288750d7a?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2664&q=80"
			alt=""
			class="h-64 w-full object-cover transition duration-500 group-hover:scale-105 sm:h-72"
		/>

		<div class="relative border border-gray-100 bg-white p-6">
			<div class="flex justify-between items-center gap-8 text-xs items-stretch">
				<div class="sm:inline-flex sm:shrink-0 sm:items-center sm:gap-2">
					<svg
						class="h-4 w-4 text-indigo-700"
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M8 14v3m4-3v3m4-3v3M3 21h18M3 10h18M3 7l9-4 9 4M4 10h16v11H4V10z"
						/>
					</svg>

					<div class="mt-1.5 sm:mt-0">
						<p class="text-gray-500">Frecuencia</p>

						<p class="font-medium">{subscription.frequency}</p>
					</div>
				</div>

				<div class="sm:inline-flex sm:shrink-0 sm:items-center sm:gap-2">
					<svg
						class="h-4 w-4 text-indigo-700"
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z"
						/>
					</svg>

					<div class="mt-1.5 sm:mt-0">
						<p class="text-gray-500">Próxima Renovación</p>

						<p class="font-medium">20 Febrero, 2024</p>
					</div>
				</div>

				<div class="sm:inline-flex sm:shrink-0 sm:items-center sm:gap-2">
					<svg
						class="h-4 w-4 text-indigo-700"
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z"
						/>
					</svg>

					<div class="mt-1.5 sm:mt-0">
						<p class="text-gray-500">Estado</p>
						<p class="font-medium">{subscription.is_active}</p>
					</div>
				</div>
			</div>

			<h3 class="mt-4 text-lg font-medium text-gray-900">{subscription.description}</h3>
			<p>Lorem</p>
			<Rating rating="4" size="24" class="mt-2.5 mb-5">
				<Badge slot="text" class="ms-3">4</Badge>
			</Rating>
			<p class="mt-1.5 text-sm text-gray-700">${subscription.price}</p>

			<form class="mt-4" on:submit|preventDefault={pay_subscription}>
				<button
					class="block w-full rounded bg-yellow-400 p-4 text-sm font-medium transition hover:scale-105 text-center"
				>
					Suscribirse
				</button>
			</form>
		</div>
	</a>
</div>
