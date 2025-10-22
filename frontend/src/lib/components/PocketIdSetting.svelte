<script lang="ts">
	import IconedResourceType from './IconedResourceType.svelte'
	import Toggle from './Toggle.svelte'

	export let value: any

	$: enabled = value != undefined

	// If `baseUrl` is not already set in the form, try to parse it from the `auth_url` value
	//
	// The binding dance here allows us to avoid rendering the string 'undefined' in the input, and
	// also allow lazy/async binding of the `value` prop.
	$: derivedBaseUrl = value?.connect_config?.auth_url?.replace('/authorize', '')
	let proxyUrlValue = undefined
	$: baseUrl = proxyUrlValue ?? derivedBaseUrl ?? ''

	$: changeBaseUrl(baseUrl)

	function changeBaseUrl(baseUrl) {
		if (value) {
			value = {
				...value,
				connect_config: {
					auth_url: `${baseUrl}/authorize`,
					token_url: `${baseUrl}/api/oidc/token`,
					scopes: ['openid', 'profile', 'email']
				},
				login_config: {
					auth_url: `${baseUrl}/authorize`,
					token_url: `${baseUrl}/api/oidc/token`,
					userinfo_url: `${baseUrl}/api/oidc/userinfo`,
					scopes: ['openid', 'profile', 'email']
				}
			}

			proxyUrlValue = baseUrl
		}
	}
</script>

<div class="flex flex-col gap-1">
	<!-- svelte-ignore a11y-label-has-associated-control -->
	<label class="text-sm font-medium text-primary flex gap-4 items-center"
		><div class="w-[120px]"><IconedResourceType name={'pocketid'} after={true} /></div><Toggle
			checked={enabled}
			on:change={(e) => {
				if (e.detail) {
					value = { id: '', secret: '' }
				} else {
					value = undefined
				}
			}}
		/></label
	>
	{#if enabled}
		<div class="border rounded p-2">
			<label class="block pb-2">
				<span class="text-primary font-semibold text-sm"
					>Pocket ID Base Url ({'POCKET_ID_URL/authorize'})</span
				>
				<input type="text" placeholder="https://your-pocket-id-instance.com" bind:value={baseUrl} />
			</label>
			<label class="block pb-2">
				<span class="text-primary font-semibold text-sm">Custom Name</span>
				<input type="text" placeholder="Custom Name" bind:value={value['display_name']} />
			</label>
			<label class="block pb-2">
				<span class="text-primary font-semibold text-sm">Client Id</span>
				<input type="text" placeholder="Client Id" bind:value={value['id']} />
			</label>
			<label class="block pb-2">
				<span class="text-primary font-semibold text-sm">Client Secret</span>
				<input type="text" placeholder="Client Secret" bind:value={value['secret']} />
			</label>
		</div>
	{/if}
</div>
