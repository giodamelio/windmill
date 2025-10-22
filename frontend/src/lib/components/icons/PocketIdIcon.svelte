<script lang="ts">
	interface Props {
		height?: string;
		width?: string;
	}

	let { height = '24px', width = '24px' }: Props = $props();
</script>

<svg {width} {height} viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
	<rect x="4" y="4" width="16" height="18" rx="2" stroke="#4F46E5" stroke-width="2" fill="none"/>
	<circle cx="12" cy="10" r="2.5" fill="#4F46E5"/>
	<path d="M8 16C8 14 9.5 13 12 13C14.5 13 16 14 16 16" stroke="#4F46E5" stroke-width="2" stroke-linecap="round"/>
	<rect x="6" y="2" width="12" height="3" rx="1" fill="#4F46E5"/>
</svg>
