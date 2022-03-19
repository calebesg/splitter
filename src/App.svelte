<script>
	import formatCurrency from "./Utils/formatCurrency";

	const tips = [5, 10, 15, 25, 50];

	let bill = "";
	let tip = "";
	let people = "";
	let amount = "";
	let total = "";

	let tipSelectedIndex;
	let resetIsVisible = false;

	const validateFields = () => {
		if (people === "" || bill === "" || tip === "") {
			throw new Error("empty");
		}

		if (people === "0") {
			throw new Error("nullable");
		}
	};

	const calculate = () => {
		try {
			validateFields();

			total = bill * (tip / 100 + 1);
			amount = total / people;

			total = formatCurrency(total);
			amount = formatCurrency(amount);

			resetIsVisible = true;
		} catch (error) {
			if (error.message == "nullable") amount = "";
		}
	};

	const setBill = (value) => {
		bill = value;
		calculate();
	};

	const setPeople = (value) => {
		people = value;
		calculate();
	};

	const setTip = (value, index = "") => {
		tip = value;
		tipSelectedIndex = index;
		calculate();
	};

	const clearData = () => {
		bill = "";
		tip = "";
		people = "";
		total = "";
		amount = "";

		tipSelectedIndex = "";
		resetIsVisible = false;
	};
</script>

<main>
	<h1 class="sr-only">Tip Calculator application</h1>
	<img class="title" src="images/logo.svg" alt="Splitter" />
	<div class="card">
		<form id="calculator">
			<div class="input-group">
				<label for="bill">Bill</label>
				<input
					id="bill"
					class="field"
					value={bill}
					type="number"
					min="1"
					step="0.1"
					placeholder="0"
					on:input={(e) => setBill(e.target.value)}
				/>
				<img
					class="dollar"
					src="images/icon-dollar.svg"
					alt=""
					aria-hidden="true"
				/>
			</div>

			<div class="input-group">
				<label for="tips">Select Tip %</label>
				<ul class="tips" id="tips">
					{#each tips as tip, index}
						<li class="tips-item">
							<button
								class={tipSelectedIndex === index && "active"}
								type="button"
								on:click={() => setTip(tip, index)}
							>
								{tip}%
							</button>
						</li>
					{/each}

					<li>
						<input
							id="custom"
							class="field"
							value={tipSelectedIndex === "" ? tip : ""}
							aria-label="Insira um valor customizado"
							type="number"
							min="1"
							step="0.5"
							placeholder="Custom"
							on:input={(e) => setTip(e.target.value)}
						/>
					</li>
				</ul>
			</div>

			<div class="input-group">
				<div class="input-header">
					<label for="peoples">Number of People</label>
					<small
						class={people === "0" ? "message-error" : "message-error hidden"}
					>
						Can`t be zero
					</small>
				</div>
				<input
					id="peoples"
					class={people === "0" ? "field error" : "field"}
					type="number"
					value={people}
					min="0"
					step="1"
					placeholder="0"
					on:input={(e) => setPeople(e.target.value)}
				/>
				<img src="images/icon-person.svg" alt="" aria-hidden="true" />
			</div>
		</form>

		<div class="display-wrap">
			<ul class="display">
				<li class="display-item">
					<h3 class="display-title">
						Tip Amount
						<small>/ person</small>
					</h3>
					<p class="display-value">
						<img src="images/icon-dollar.svg" alt="" aria-hidden="true" />
						<span>{amount ? amount : "0.00"}</span>
					</p>
				</li>
				<li class="display-item">
					<h3 class="display-title">
						Total
						<small>/ person</small>
					</h3>
					<p class="display-value">
						<img src="images/icon-dollar.svg" alt="" aria-hidden="true" />
						<span>{total ? total : "0.00"}</span>
					</p>
				</li>
			</ul>

			<button
				type="reset"
				form="calculator"
				disabled={resetIsVisible ? "" : "disabled"}
				on:click={clearData}
			>
				RESET
			</button>
		</div>
	</div>
</main>

<style>
	main {
		min-height: 100vh;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.sr-only {
		widows: 1px;
		height: 1px;
		opacity: 0;
		overflow: hidden;
	}

	.title {
		margin: 2.5rem 0;
		color: var(--dark-grayish-cyan);
		letter-spacing: 1rem;
		font-size: 1.5rem;
	}

	.card {
		width: 100%;
		padding: 2rem 1.5rem;
		background-color: var(--white);
		border-top-left-radius: 1.25rem;
		border-top-right-radius: 1.25rem;
	}

	#calculator {
		padding: 0.5rem 0.5rem 0;
	}

	.input-group + .input-group {
		margin-top: 2.25rem;
	}

	.input-group label {
		color: var(--dark-grayish-cyan);
		margin-bottom: 0.625rem;
		display: inline-block;
	}

	.input-header {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.input-header .message-error {
		color: #cc7763;
	}

	.message-error.hidden {
		opacity: 0;
		display: none;
	}

	.input-group {
		position: relative;
	}

	.input-group img {
		position: absolute;
		left: 1rem;
		bottom: 1rem;
	}

	.input-group img.dollar {
		filter: grayscale(81%);
	}

	.input-group .field {
		text-align: right;
		font-size: 1.5rem;
		padding-right: 1rem;
		color: var(--dark-grayish-cyan);
		background-color: var(--very-light-grayish-cyan);

		appearance: textfield;
		-webkit-appearance: textfield;
		-moz-appearance: textfield;
	}

	.input-group .field:focus {
		cursor: pointer;
		border: 3px solid var(--strong-cyan);
		outline: transparent;
	}

	.input-group .field.error {
		border: 3px solid #cc7763;
	}

	.input-group .field::placeholder {
		color: var(--dark-grayish-cyan);
	}

	.tips {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: repeat(3, 3rem);
		gap: 1rem;
	}

	.tips button {
		width: 100%;
		height: 100%;
		background-color: var(--very-dark-cyan);
		color: var(--white);
		border-radius: 0.25rem;
		cursor: pointer;
		transition: all 0.3s;
	}

	.tips button:hover {
		color: var(--very-dark-cyan);
		background-color: var(--strong-cyan);
	}

	.tips button.active {
		background-color: #9fe8df;
		color: var(--very-dark-cyan);
	}

	.tips .field::placeholder {
		color: var(--very-dark-cyan);
		opacity: 0.8;
	}

	.display-wrap {
		margin-top: 2rem;
		background-color: var(--very-dark-cyan);
		border-radius: 0.875rem;
		padding: 1.5rem;
		padding-top: 2.875rem;
	}

	.display .display-item {
		display: flex;
		justify-content: space-between;
		align-items: center;
		margin-bottom: 1.875rem;
	}

	.display-item .display-title {
		color: var(--white);
		font-size: 1rem;
		display: flex;
		flex-direction: column;
		align-items: flex-start;
	}

	.display-title small {
		color: var(--grayish-cyan);
	}

	.display-value {
		font-size: 2rem;
		color: var(--strong-cyan);
		display: flex;
		align-items: center;
	}

	.display-value img {
		height: 1.5rem;
		margin-right: 0.25rem;
	}

	.display-wrap button {
		background-color: var(--strong-cyan);
		color: var(--very-dark-cyan);
		transition: background-color 0.3s;
	}

	.display-wrap button:not(:disabled):hover {
		cursor: pointer;
		background-color: #9fe8df;
	}

	.display-wrap button:disabled {
		opacity: 0.3;
	}

	@media (min-width: 60rem) {
		.card {
			width: 57.5rem;
			border-radius: 1.25rem;
			padding: 2rem;
			display: grid;
			grid-template-columns: 1fr 1fr;
			gap: 2rem;
		}

		#calculator {
			padding: 1rem;
		}

		.title {
			margin-top: 0;
			margin-bottom: 80px;
		}

		.input-group .field {
			padding-right: 0;
		}

		.tips {
			grid-template-columns: repeat(3, 1fr);
			grid-template-rows: 3rem 3rem;
			gap: 1rem;
		}

		.display-wrap {
			padding: 3.875rem 2.5rem 2.5rem;
			margin-top: 0;
			display: flex;
			flex-direction: column;
			justify-content: space-between;
		}

		.display-value {
			font-size: 3rem;
		}

		.display-value img {
			height: 2.5rem;
			margin-right: 0.25rem;
		}
	}
</style>
