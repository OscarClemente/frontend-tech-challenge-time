<script>
	import Timers from './components/Timers.svelte'
	import {initClient} from "@urql/svelte"

	let timers = [{id: 1, title: "Example timer 1"},
								{id: 2, title: "Example timer 2"},]
</script>

<Timers {timers}/>