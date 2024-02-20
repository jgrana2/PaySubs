<script>
	import HeaderAvatar from '$lib/components/HeaderAvatar.svelte';
	import Sidebar from '$lib/components/Sidebar.svelte';
	import SubscribersTable from '$lib/components/SubscribersTable.svelte';
</script>

<div class="flex bg-gray-50">
	<Sidebar />
	<div class="flex flex-col w-full px-4 h-screen"> <!-- Set a height for the container -->
	  <HeaderAvatar title="Suscriptores" /> <!-- This will stay fixed at the top -->
	  <div class="overflow-auto"> <!-- This div will be scrollable -->
		<SubscribersTable />
	  </div>
	</div>
  </div>
  
