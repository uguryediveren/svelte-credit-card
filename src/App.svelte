<script>
	import Inputmask from "inputmask";
	import creditCardType from "credit-card-type";

	let backface = false;
	let numberInput, expireInput, cvvInput;
	let card = {
		number: "",
		name: "",
		expiry: "",
		cvv: "",
	};
	//4543visa 5269mastercard
	// $: type = card.number ? creditCardType(card.number)?.[0]?.type : false;
	$: type = card.number ? creditCardType(card.number)?.[0]?.type : false;

	$: console.log("tt", type);

	$: if (numberInput && cvvInput && expireInput) {
		Inputmask({
			mask: "9999 9999 9999 9999",
			placeholder: " ",
			showMaskOnHover: false,
		}).mask(numberInput);

		Inputmask({
			mask: "99/99",
			placeholder: " ",
			showMaskOnHover: false,
		}).mask(expireInput);

		Inputmask({
			mask: "999",
			placeholder: " ",
			showMaskOnHover: false,
		}).mask(cvvInput);
	}

	const handleAdd = () => {
		if (card.name && card.number && card.expiry && card.cvv) {
			alert("Card added \n" + JSON.stringify(card));
			console.log("card", card);
		} else return;
	};
</script>

<div class="container">
	<div class="card" class:flip={backface}>
		<div class="front">
			<div class="card-top">
				<img src="img/chip.svg" alt="chip" />
				{#if type}
					<img src="img/{type}.svg" alt={type} />
				{/if}
			</div>
			<div class="card-number">
				{card.number || "**** **** **** ****"}
			</div>
			<div class="card-bottom">
				<div>
					<div class="key">Card Holder Name</div>
					<div class="value">{card.name || "***** *****"}</div>
				</div>
				<div>
					<div class="key">Expiry Date</div>
					<div class="value">{card.expiry || "**/**"}</div>
				</div>
			</div>
		</div>

		<div class="back">
			<div class="card-back">
				CVV <em>{card.cvv || "***"}</em>
			</div>
		</div>
	</div>

	<input
		type="text"
		bind:this={numberInput}
		placeholder="Card Number"
		bind:value={card.number}
	/>
	<input
		type="text"
		pattern="[a-zA-Z'-'\s]*"
		placeholder="Card Holder Name"
		bind:value={card.name}
	/>
	<input
		type="text"
		bind:this={expireInput}
		placeholder="Expire Date"
		bind:value={card.expiry}
	/>
	<input
		type="text"
		on:focus={() => (backface = true)}
		on:blur={() => (backface = false)}
		bind:this={cvvInput}
		placeholder="CVV"
		bind:value={card.cvv}
	/>
	<label>
		<input type="checkbox" bind:checked={backface} />
		{backface ? "Hide" : "Show"} backside
	</label> <br />

	<button on:click={handleAdd}>Add</button>

	<!-- <pre>{JSON.stringify(backface)}</pre>
	<pre>{JSON.stringify(card)}</pre>
	<pre>{JSON.stringify(type)}</pre> -->
</div>

<style>
	/* input {
		margin: 0 auto;
	} */

	.container {
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
		flex-direction: column;
	}

	:global(body) {
		font-family: sans-serif;
		background-color: rgb(148, 162, 187);
		/* center div  */
	}
	.card {
		width: 352px;
		height: 223px;
		position: relative;
		perspective: 800px;
		margin-bottom: 20px;
	}

	.card .front .card-number {
		font-size: 28px;
		font-family: monospace;
		color: #fff;
		letter-spacing: -3px;
		margin-top: 45px;
	}

	.card .front,
	.card .back {
		width: inherit;
		height: inherit;
		box-sizing: border-box;
		background: linear-gradient(31.58deg, #93278f -2.49%, #29abe2 67.92%);
		border-radius: 15px;
		position: absolute;
		top: 0;
		left: 0;
		transition: 1s all;
		backface-visibility: hidden;
		padding: 31px 27px;
		display: flex;
		flex-direction: column;
	}

	.card .back {
		transform: rotateY(180deg);
	}

	.card .front .card-top {
		display: flex;
		justify-content: space-between;
	}

	.card .front .card-bottom .key {
		font-size: 11px;
		font-weight: 500;
		text-transform: uppercase;
		margin-bottom: 5px;
		opacity: 0.7;
	}

	.card .front .card-bottom .value {
		font-size: 18px;
		font-weight: 500;
	}

	.card .front .card-bottom {
		margin-top: auto;
		display: flex;
		justify-content: space-between;
		color: #fff;
	}

	.card.flip .back {
		transform: rotateY(0);
	}

	.card.flip .front {
		transform: rotateY(-180deg);
	}

	.card .back .card-back {
		background-color: #fff;
		padding: 20px;
		margin-top: auto;
		display: flex;
		justify-content: end;
	}

	.card .back .card-back em {
		font-weight: bold;
		margin-left: 15px;
	}

	input::-webkit-outer-spin-button,
	input::-webkit-inner-spin-button {
		-webkit-appearance: none;
		margin: 0;
	}

	button {
		border-radius: 5px;
		width: 220px;
		background-color: white;
	}
</style>
