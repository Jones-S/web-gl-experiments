<script>
	import Nav from '../components/Nav/Nav.svelte'

	export let segment
</script>

<Nav {segment}/>

<main>
	<slot></slot>
</main>
