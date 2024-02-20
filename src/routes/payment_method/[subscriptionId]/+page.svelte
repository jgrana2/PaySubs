<script lang="ts">
	import Header from '$lib/components/Header.svelte';
	import { LockClosed } from 'svelte-heros-v2';
	import valid from 'card-validator';
	import { onMount } from 'svelte';
	import { saveCard } from '$lib/appwrite';
	import user from '../../../store/user';

	let creditCardNumber = ''; // Initialize a reactive variable to store the credit card number
	let expiration = ''; // Initialize a reactive variable to store the expiration date
	let creditCardElement: HTMLInputElement; // Reference to store the input element for the card number
	let cvc = '';
	let card_type = '';
	let subscription = {}; // Create a variable for the subscription data
	let loggedInUser = {}; // Initialize loggedInUser as an empty object
	let isLoggedIn = false;

	// Subscription ID from URL parameter
	export let data;

	user.subscribe((user) => {
		loggedInUser = user;
		// Check if loggedInUser is not null and has properties (not an empty object)
		isLoggedIn = loggedInUser && Object.keys(loggedInUser).length > 0;
	});

	onMount(async () => {
		validateCard(creditCardNumber); // Optional: Validate immediately on mount if pre-filled

		// Fetch subscription data
		try {
			const response = await fetch(`http://127.0.0.1:5000/subscription/${data.id}`);
			if (response.ok) {
				const jsonData = await response.json();
				subscription = jsonData;
			}
		} catch (error) {
			console.error('There was an error fetching subscription data:', error);
		}
	});

	function validateCardNumber(event: Event) {
		const target = event.target as HTMLInputElement;
		creditCardNumber = target.value;
		if (validateCard(creditCardNumber)) {
			console.log('valid');
			target.setCustomValidity('');
		} else {
			console.log('invalid');
			target.setCustomValidity('Invalid card number.');
		}
		target.reportValidity();
	}

	async function handleSubmit(event: SubmitEvent & { currentTarget: HTMLFormElement }) {
		event.preventDefault();
		const formData = new FormData(event.currentTarget); // Should be currentTarget, not target

		if (validateCard(formData.get('card_number'))) {
			// Guardar tarjeta en base de datos
			const card_data = {
				card_number: formData.get('card_number'),
				card_holder: formData.get('card_holder'),
				card_holder_id_number: formData.get('document_id'),
				card_holder_id_type: "CC",
				expiration: formData.get('expiration'),
				cvc: formData.get('cvc')
			};

			try {
				await saveCard(card_data);
			} catch (error) {
				console.log('Error guardando la tarjeta.');
			}

			// Process payment
			try {
				const response = await fetch('http://127.0.0.1:5000/activate-subscription', {
					method: 'POST',
					headers: {
						'Content-Type': 'application/json'
					},
					body: JSON.stringify({
						subscription_id: data.id,
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

				const response_data = await response.json();
				console.log(response_data);
				window.location.href = '../success';
			} catch (error) {
				console.error('There was an error activating the subscription:', error);
			}
		}
	}

	function validateCard(cardNumber: string): boolean {
		const numberValidation = valid.number(cardNumber);

		if (numberValidation.card) {
			card_type = numberValidation.card.type; // Log card type such as 'visa'
		}

		if (!numberValidation.isPotentiallyValid || !numberValidation.isValid) {
			return false; // Return false to indicate the card is not valid.
		}
		return true; // The card is valid, so return true.
	}

	// This function handles the input and automatically inserts a slash when needed
	function formatExpirationInput(event) {
		let target = event.target;
		let value = target.value;

		// Remove all non-digit characters
		let cleanValue = value.replace(/\D+/g, '');

		// Extract potential month and year parts
		let monthPart = cleanValue.substring(0, 2);
		let yearPart = cleanValue.substring(2);

		// Parse the month and year to see if they're valid numbers
		let monthNumber = parseInt(monthPart, 10);
		let currentYear = new Date().getFullYear();
		let shortYear = currentYear % 100; // Get last two digits of current year for comparison
		let enteredYear = parseInt(yearPart, 10);

		// Check if the month is valid (1-12)
		if (monthNumber < 1 || monthNumber > 12) {
			target.setCustomValidity('Invalid month. Please enter a value between 01 and 12.');
		} else if (enteredYear < shortYear || enteredYear > shortYear + 10) {
			// Here we check if the year is in the past or too far in the future; adjust the range as needed
			target.setCustomValidity(
				`Invalid year. Please enter a value between ${shortYear} and ${shortYear + 10}.`
			);
		} else {
			// If both month and year are valid, clear any custom validity messages
			target.setCustomValidity('');
		}

		target.reportValidity();

		// Automatically add slash if length is 2 and previous length was less than 2
		if (cleanValue.length === 2 && value.length < 5) {
			target.value = `${cleanValue}/`;
		} else if (cleanValue.length <= 4) {
			// Reformat the string in MM/YY format if it has 3 or 4 digits
			target.value = `${monthPart}${yearPart.length > 0 ? '/' + yearPart : ''}`;
		}

		// Update the reactive variable after formatting
		expiration = target.value;
	}

	// This function validates that the CVC is at most 4 digits long
	function validateCVCInput(event) {
		const inputElement = event.target;
		cvc = inputElement.value;

		// First, clear any old validity messages
		inputElement.setCustomValidity('');

		// Check if the cleaned CVC input exceeds 4 digits
		if (!/^\d{3,4}$/.test(cvc)) {
			// If it does, set a custom validity message
			inputElement.setCustomValidity('CVC must be 3 or 4 digits long');
		}

		// Report to the containing form that the input might be invalid
		// (this will prevent form submission until the error is corrected)
		inputElement.reportValidity();
	}
</script>

<Header></Header>

<div class="flex flex-col max-w-screen-sm mx-auto">
	<div class="text-lg font-bold pb-4 w-full px-4">Agregar Método de Pago</div>
	<form on:submit|preventDefault={handleSubmit} class="w-full p-4">
		<div class="flex justify-between">
			<div class="text-sm py-2">Número de tarjeta</div>
			<span class="inline-flex items-center rounded-md px-2 py-1 text-xs font-medium text-gray-600"
				>{card_type}</span
			>
		</div>
		<div class="flex flex-col sm:flex-row items-center border-b mb-4 border-gray-500">
			<input
				bind:this={creditCardElement}
				class="appearance-none bg-transparent border-none w-full sm:w-2/4 text-gray-700 mb-4 sm:mb-0 sm:mr-3 py-1 px-2 leading-tight focus:outline-none"
				type="tel"
				placeholder="1234 5678 9012 3456"
				inputmode="numeric"
				name="card_number"
				bind:value={creditCardNumber}
				on:input={validateCardNumber}
				required
			/>
			<input
				class="appearance-none bg-transparent border-none w-full sm:w-1/4 text-gray-700 mb-4 sm:mb-0 sm:mr-3 py-1 px-2 leading-tight focus:outline-none"
				type="text"
				placeholder="MM/AA"
				name="expiration"
				bind:value={expiration}
				on:input={formatExpirationInput}
				maxlength="5"
				required
			/>
			<input
				class="appearance-none bg-transparent border-none w-full sm:w-1/4 text-gray-700 mb-4 sm:mb-0 sm:mr-3 py-1 px-2 leading-tight focus:outline-none"
				type="text"
				placeholder="CVC"
				maxlength="4"
				pattern="\d*"
				name="cvc"
				on:input={validateCVCInput}
				required
			/>
		</div>
		<div class="text-sm py-2">Nombre del titular</div>
		<div class="flex items-center border-b mb-4 border-gray-500">
			<input
				class="appearance-none bg-transparent border-none w-full text-gray-700 mb-4 sm:mb-0 sm:mr-3 py-1 px-2 leading-tight focus:outline-none"
				type="text"
				placeholder="María Lopez"
				name="card_holder"
				required
			/>
		</div>
		<div class="text-sm py-2">Documento del titular</div>
		<div class="flex items-center border-b mb-4 border-gray-500">
			<input
				class="appearance-none bg-transparent border-none w-full text-gray-700 mb-4 sm:mb-0 sm:mr-3 py-1 px-2 leading-tight focus:outline-none"
				type="number"
				placeholder="123456789"
				name="document_id"
				required
			/>
		</div>
		<div class="text-sm py-2">E-mail</div>
		<div class="flex items-center border-b mb-4 border-gray-500">
			<input
				class="appearance-none bg-transparent border-none w-full text-gray-700 mb-4 sm:mb-0 sm:mr-3 py-1 px-2 leading-tight focus:outline-none"
				type="email"
				placeholder="marialopez@ejemplo.com"
				name="email"
				required
			/>
		</div>
		<button
			type="submit"
			class="w-full justify-center bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold py-2 px-4 rounded inline-flex items-center"
		>
			<svelte:component this={LockClosed} size="15" class="flex-shrink-0 mr-2" />
			<span>Pagar</span>
		</button>
	</form>
</div>
