<script>
	// SvelteKit script tag for any JavaScript you might need

	import { ImagePlaceholder } from 'flowbite-svelte';
</script>

<svelte:head>
	<title>Welcome to PaySubs!</title>
</svelte:head>

<div class="bg-gray-100">
	<div class="container mx-auto py-16 text-center">
		<div class="flex justify-center">
			<img src="logo.png" alt="Logo" class="w-16" />
		</div>

		<h1 class="text-4xl font-bold mb-4">Welcome to PaySubs!</h1>
		<p class="text-lg text-gray-600">Simple. Direct. Subscriptions Done Right.</p>

		<div class="mt-12">
			<h2 class="text-2xl font-semibold mb-4">
				Are you ready to monetize your content or services?
			</h2>
			<p class="text-gray-700">
				PaySubs makes it easy, effective, and secure for creators like you to set up subscription
				packages and share them with the world.
			</p>
		</div>

		<div class="mt-12 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
			<!-- Create Your Package -->
			<div class="p-6 bg-white rounded shadow">
				<h3 class="text-xl font-semibold mb-2">Create Your Package</h3>
				<p class="text-gray-600">
					Craft tailored subscription options that fit your business model. Set your price,
					duration, and content access levels with ease.
				</p>
			</div>

			<!-- Share Effortlessly -->
			<div class="p-6 bg-white rounded shadow">
				<h3 class="text-xl font-semibold mb-2">Share Effortlessly</h3>
				<p class="text-gray-600">
					Generate unique links for your subscription packages. Spread the word through social
					media, emails, or on your website.
				</p>
			</div>

			<!-- Seamless Subscriber Experience -->
			<div class="p-6 bg-white rounded shadow">
				<h3 class="text-xl font-semibold mb-2">Seamless Subscriber Experience</h3>
				<p class="text-gray-600">
					Users can view subscription details with a single click. A straightforward process guides
					them from interest to subscription.
				</p>
			</div>
		</div>

		<div class="mt-12">
			<h2 class="text-2xl font-semibold mb-4">New Users: Get Started in Moments</h2>
			<p class="text-gray-700">
				Simple sign-up process to get you going. A user-friendly interface that'll have you
				subscribed in no time.
			</p>
		</div>

		<div class="mt-12">
			<h2 class="text-2xl font-semibold mb-4">Secure Payment</h2>
			<p class="text-gray-700">
				Add your payment method without hassle. Trust in leading gateways like MercadoPago for your
				transactions.
			</p>
		</div>

		<div class="mt-12">
			<h2 class="text-2xl font-semibold mb-4">Instant Access</h2>
			<p class="text-gray-700">
				Hit the 'Subscribe' button and dive into your new content immediately. Enjoy being part of
				an ever-growing community of subscribers.
			</p>
		</div>

		<div class="mt-12 flex justify-center">
			<img src="subsflow.svg" alt="Subscription Flow Diagram" />
		</div>

		<div class="mt-12">
			<p class="text-xl font-semibold mb-4">
				Join <span class="text-blue-500">PaySubs</span> today – where subscribing is just as enjoyable
				as what you're subscribing to! 🌟
			</p>
			<p class="mb-8">Ready to unlock exclusive content?</p>
			<a
				href="http://localhost:5173/register/65babc7cdc55a9841e0f"
				class="bg-blue-500 text-white py-2 px-4 rounded hover:bg-blue-600">Register Now</a
			>
			<p class="mt-4">
				Have an account already? <a href="http://localhost:5173/login" class="text-blue-500"
					>Login</a
				>
			</p>
		</div>
	</div>

	<div class="bg-gray-200 text-center py-4 mt-8">
		<p class="text-sm text-gray-600">
			For support or inquiries, please contact us at <a
				href="mailto:support@PaySubs.com"
				class="text-blue-500">support@PaySubs.com</a
			>.
		</p>
	</div>
</div>
