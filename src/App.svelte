<script>
	import Timers from './components/Timers.svelte';
	import {initClient} from '@urql/svelte';

	let timers = [];
	
	initClient({
		url: "http://localhost:8080/query"
	});
</script>

<Timers {timers}/>