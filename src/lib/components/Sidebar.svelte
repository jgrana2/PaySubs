<script>
	import Logo from "./Logo.svelte";
	import { UserGroup, CalendarDays, Briefcase, Home } from 'svelte-heros-v2';
</script>
<div class="flex flex-col min-w-52 h-screen bg-gray-200 hidden lg:block">
	<Logo></Logo>
	<button class="flex w-full justify-left h-16 items-center px-4 rounded hover:bg-slate-300">
		<svelte:component this="{Home}" size="25" class="flex-shrink-0"/> 
		<span class="pl-4">Inicio</span>
	</button>
	<a href="subscribers" class="flex w-full justify-left h-16 items-center px-4 rounded hover:bg-slate-300">
		<svelte:component this="{UserGroup}" size="25" class="flex-shrink-0"/> 
		<span class="pl-4">Suscriptores</span>
	</a>
	<a href="subscriptions" class="flex w-full justify-left h-16 items-center px-4 rounded hover:bg-slate-300">
		<svelte:component this="{CalendarDays}" size="25" class="flex-shrink-0"/> 
		<span class="pl-4">Suscripciones</span>
	</a>
	<button class="flex w-full justify-left h-16 items-center px-4 rounded hover:bg-slate-300">
		<svelte:component this="{Briefcase}" size="25" class="flex-shrink-0"/> 
		<span class=" pl-4">Métodos de Pago</span>
	</button>
	  
</div>