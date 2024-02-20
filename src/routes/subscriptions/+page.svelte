<script>
	import HeaderAvatar from '$lib/components/HeaderAvatar.svelte';
	import Sidebar from '$lib/components/Sidebar.svelte';
	import SubscriptionsTable from '$lib/components/SubscriptionsTable.svelte';
	import AddSubscription from '$lib/components/AddSubscription.svelte'; // Add this import for the AddSubscription
</script>

<div class="flex bg-gray-50">
	<Sidebar />
	<div class="flex flex-col w-full px-4 h-screen">
	  <HeaderAvatar title="Suscripciones" />
	  <AddSubscription />
	  <div class="overflow-auto">
		<SubscriptionsTable />
	  </div>
	</div>
  </div>