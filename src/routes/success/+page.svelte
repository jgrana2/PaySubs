<script>
	import {
		Avatar,
		Dropdown,
		DropdownHeader,
		DropdownItem,
		DropdownDivider,
		Rating,
		Badge
	} from 'flowbite-svelte';
</script>

<nav class="bg-white border-gray-200 dark:bg-gray-900">
	<div class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto p-4">
		<a href="#" class="flex items-center space-x-3 rtl:space-x-reverse">
			<img src="logo.jpg" class="h-8" alt="PayGate Logo" />
			<span class="self-center text-2xl font-semibold whitespace-nowrap dark:text-white"
				>PayGate</span
			>
		</a>

		<div class="flex items-cenier md:order-2 md:space-x-0 rtl:space-x-reverse">
			<Avatar id="user-drop" class="cursor-pointer" dot={{ color: 'green' }} />
			<Dropdown triggeredBy="#user-drop">
				<DropdownHeader>
					<span class="block text-sm">John Doe</span>
					<span class="block truncate text-sm font-medium">john@doe.com</span>
				</DropdownHeader>
				<DropdownItem href="manage_subscriptions">Gestionar suscripciones</DropdownItem>
				<DropdownItem href="manage_cards">Métodos de pago</DropdownItem>
				<DropdownDivider />
				<DropdownItem>Cerrar Sesión</DropdownItem>
			</Dropdown>
			<!-- Dropdown menu -->
			<div
				class="z-50 hidden my-4 text-base list-none bg-white divide-y divide-gray-100 rounded-lg shadow dark:bg-gray-700 dark:divide-gray-600"
				id="user-dropdown"
			>
				<div class="px-4 py-3">
					<span class="block text-sm text-gray-900 dark:text-white">Bonnie Green</span>
					<span class="block text-sm text-gray-500 truncate dark:text-gray-400"
						>name@flowbite.com</span
					>
				</div>
				<ul class="py-2" aria-labelledby="user-menu-button">
					<li>
						<a
							href="#"
							class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white"
							>Dashboard</a
						>
					</li>
					<li>
						<a
							href="#"
							class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white"
							>Settings</a
						>
					</li>
					<li>
						<a
							href="#"
							class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white"
							>Earnings</a
						>
					</li>
					<li>
						<a
							href="#"
							class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-200 dark:hover:text-white"
							>Sign out</a
						>
					</li>
				</ul>
			</div>
		</div>
	</div>
</nav>

<div class="bg-gray-100 h-screen">
    <div class="bg-white p-6  md:mx-auto">
      <svg viewBox="0 0 24 24" class="text-green-600 w-16 h-16 mx-auto my-6">
          <path fill="currentColor"
              d="M12,0A12,12,0,1,0,24,12,12.014,12.014,0,0,0,12,0Zm6.927,8.2-6.845,9.289a1.011,1.011,0,0,1-1.43.188L5.764,13.769a1,1,0,1,1,1.25-1.562l4.076,3.261,6.227-8.451A1,1,0,1,1,18.927,8.2Z">
          </path>
      </svg>
      <div class="text-center">
          <h3 class="md:text-2xl text-base text-gray-900 font-semibold text-center">Payment Done!</h3>
          <p class="text-gray-600 my-2">Thank you for completing your secure online payment.</p>
          <p> Have a great day!  </p>
          <div class="py-10 text-center">
              <a href="login" class="px-12 bg-indigo-600 hover:bg-indigo-500 text-white font-semibold py-3">
                  GO BACK 
             </a>
          </div>
      </div>
  </div>
</div>