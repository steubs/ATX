<script lang="ts">
	import { slide } from 'svelte/transition';
	import { faTwitch } from '@fortawesome/free-brands-svg-icons';
	import Fa from 'svelte-fa';
	import CurrencyView from './CurrencyView.svelte';
	import NetworthView from './NetworthView.svelte';

	import { loggedIn, profile } from '$lib/stores/userData';
	import { loginDialog } from '$lib/stores/uiStates';
	import { logout } from '$lib/supabase';

	let isDropdownVisible: boolean = false;

	function toggleDropdown(e: Event) {
		isDropdownVisible = !isDropdownVisible;
	}

	function handleLogout() {
		logout();
		isDropdownVisible = false;
	}
</script>

{#if $loggedIn}
	<div class="sm:hidden {isDropdownVisible == true ? 'hidden' : 'block'}">
		<NetworthView />
	</div>
{/if}

<div class="flex flex-row justify-between items-center">
	{#if $loggedIn}
		<div class="flex flex-row items-center mr-5 max-sm:hidden">
			<CurrencyView />
			<NetworthView />
		</div>
		<button class="max-sm:hidden flex flex-row items-center" on:click={toggleDropdown}>
			<!-- This is the anchor that can open the dropdown.-->
			<div class="flex flex-row items-center space-x-2">
				<span class="text-light-text dark:text-dark-text">{$profile?.username}</span>
				<img
					src={$profile?.image}
					class="rounded-full border-solid border-2 border-light-secondary dark:border-dark-secondary object-fill h-10"
					alt="The user's twitch profile."
				/>
			</div>
		</button>
		<button
			on:click={toggleDropdown}
			type="button"
			class="sm:hidden text-light-text focus:text-light-secondary dark:text-dark-text dark:focus:text-dark-secondary focus:outline-none"
		>
			<svg class="h-8 w-8 fill-current" viewBox="0 0 24 24">
				<path
					class={isDropdownVisible == true ? 'block' : 'hidden'}
					fill-rule="evenodd"
					d="M18.278 16.864a1 1 0 0 1-1.414 1.414l-4.829-4.828-4.828 4.828a1 1 0 0 1-1.414-1.414l4.828-4.829-4.828-4.828a1 1 0 0 1 1.414-1.414l4.829 4.828 4.828-4.828a1 1 0 1 1 1.414 1.414l-4.828 4.829 4.828 4.828z"
				/>
				<path
					class={isDropdownVisible == false ? 'block' : 'hidden'}
					fill-rule="evenodd"
					d="M4 5h16a1 1 0 0 1 0 2H4a1 1 0 1 1 0-2zm0 6h16a1 1 0 0 1 0 2H4a1 1 0 0 1 0-2zm0 6h16a1 1 0 0 1 0 2H4a1 1 0 0 1 0-2z"
				/>
			</svg>
		</button>
		{#if isDropdownVisible}
			<div
				class="max-sm:hidden absolute top-[68px] right-0 bg-light-background dark:bg-dark-background dark:border-dark-secondary rounded-bl-lg drop-shadow-lg border-0 dark:border-l-[1px] dark:border-b-[1px]"
				transition:slide={{ delay: 0, duration: 300 }}
			>
				<div class="flex flex-col items-start">
					<a
						class="text-light-text hover:text-light-primary hover:bg-light-background dark:text-dark-text dark:hover:text-dark-primary dark:hover:bg-dark-background cursor-pointer w-full"
						href="/profile/{$profile?.username}"><div class="m-2">Profile</div></a
					>
					<a
						class="text-light-text hover:text-light-primary hover:bg-light-background dark:text-dark-text dark:hover:text-dark-primary dark:hover:bg-dark-background cursor-pointer w-full"
						href="/settings"><div class="m-2">Account Settings</div></a
					>
					<div
						class="text-light-text hover:text-light-primary hover:bg-light-background dark:text-dark-text dark:hover:text-dark-primary dark:hover:bg-dark-background w-full p-2 rounded-b-lg cursor-pointer"
					>
						<button on:click={handleLogout}>Sign Out</button>
					</div>
				</div>
			</div>
			<div
				class="sm:hidden absolute top-12 right-0 px-4 pb-4 bg-light-background dark:bg-dark-background dark:border-dark-secondary rounded-bl-lg border-0 dark:border-l-[1px] dark:border-b-[1px]"
				transition:slide={{ delay: 0, duration: 300 }}
			>
				<div class="flex items-center pt-2">
					<img
						class="h-8 w-8 rounded-full border-2 border-light-secondary dark:border-dark-secondary object-cover"
						src={$profile?.image}
						alt="The user's twitch profile."
					/>
					<span class="ml-3 font-semibold text-light-text dark:text-dark-text"
						>{$profile?.username}</span
					>
				</div>
				<div
					class="flex flex-col mt-3 py-2 border-t border-light-secondary dark:border-dark-secondary"
				>
					<CurrencyView />
					<NetworthView />
				</div>
				<div class="pt-2 border-t border-light-secondary dark:border-dark-secondary">
					<a
						href="/profile/{$profile?.username}"
						class="block text-light-text hover:text-light-primary dark:text-dark-text dark:hover:text-dark-primary"
						>Profile</a
					>
					<a
						href="/settings"
						class="mt-2 block text-light-text hover:text-light-primary dark:text-dark-text dark:hover:text-dark-primary"
						>Account settings</a
					>
					<button
						on:click={handleLogout}
						class="mt-2 block text-light-text hover:text-light-primary dark:text-dark-text dark:hover:text-dark-primary"
					>
						Sign out
					</button>
				</div>
			</div>
		{/if}
		<!-- This is where the dropdown goes, that has more settings. -->
	{:else}
		<button
			class="bg-twitch text-dark-text rounded p-2 flex-row flex items-center"
			on:click={() => ($loginDialog = true)}
		>
			<Fa icon={faTwitch} class="mr-2" />
			<span>Login via Twitch</span>
		</button>
	{/if}
</div>
