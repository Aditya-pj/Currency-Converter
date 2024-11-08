<script>
	import { onMount } from "svelte";
	let currArray = $state({});
	let val1 = $state(0);
	let val2 = $state(0);
	let valCurr1 = $state("1000SATS");
	let valCurr2 = $state("1000SATS");
	let ConvVal1 = $derived(currArray[valCurr1.toLowerCase()]);
	let ConvVal2 = $derived(currArray[valCurr2.toLowerCase()]);
	onMount(async () => {
		fetch("https://latest.currency-api.pages.dev/v1/currencies/usd.json")
		.then(response => response.json())
		.then(data => {
			console.log(data);
			currArray = data.usd;
			console.log($state.snapshot(currArray))
		}).catch(error => {
			console.log(error);
		})
	});

	function handleinput(event) {
		const currInputNo = event.target.id;
		const convFactor = (currInputNo == "in1") ? ConvVal2/ConvVal1 : ConvVal1/ConvVal2;
		const currUserNo = (currInputNo == "in1") ? val1 : val2;
		const ans = convFactor * currUserNo;
		if (currInputNo == "in1") {
			val2 = ans.toFixed(3);
		} else {
			val1 = ans.toFixed(3);
		}
	}
</script>

<main>
	<h1>Currency Converter</h1>

<div class="input-boxes">
	<div class="input-box">

		<input id="in1" type="number" bind:value={val1} oninput={handleinput}/>
		<select id="s1" bind:value={valCurr1} onchange={handleinput}>
			{#each Object.entries(currArray) as [curr,val]}
				<option>{curr.toUpperCase()}</option>
			{/each}
		</select>
	</div>
	<div class="input-box">
		<input id="in2" type="number" bind:value={val2} oninput={handleinput}/>
		<select id="s2" bind:value={valCurr2} onchange={handleinput}>
			{#each Object.entries(currArray) as [curr,val]}
				<option>{curr.toUpperCase()}</option>
			{/each}
		</select>
	</div>
</div>
</main>


<style>
	.input-boxes {
		display: flex;
		justify-content: space-evenly;
		padding-bottom: 100px;
	}

	input {
		padding: 8px;
	}

	.input-box {
		margin: 0px;

	}

	select {
		padding: 8px;
	}

	h1 {
		color: aqua;
        margin-top: 100px;
        margin-bottom: 100px;
		text-align: center;
		font-size: 40px;
	}
</style>
