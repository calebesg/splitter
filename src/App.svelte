<script>
	import formatCurrency from "./Utils/formatCurrency";

	const values = {
		bill: "",
		tips: "",
		peoples: "",
	};
	let amount = 0;
	let total = 0;

	const taxes = [5, 10, 15, 25, 50];

	let resetIsVisible = false;

	function cleanForm() {
		values.bill = "";
		values.tips = "";
		values.peoples = "";

		total = 0;
		amount = 0;

		resetIsVisible = false;
	}

	function changeValue(field, value) {
		values[field] = Number(value);
		calculateTip();
	}

	function calculateTip() {
		try {
			validateFields();

			const { bill, tips, peoples } = values;
			total = bill * (tips / 100 + 1);
			amount = total / peoples;

			total = formatCurrency(total);
			amount = formatCurrency(amount);

			resetIsVisible = true;
		} catch (error) {
			if (error.message == "nullable") amount = 0;
		}
	}

	function validateFields() {
		const { bill, tips, peoples } = values;

		if (peoples === "" || bill === "" || tips === "") {
			throw new Error("empty");
		}

		if (peoples === 0) {
			throw new Error("nullable");
		}
	}
</script>

<main>
	<h1 class="title">SPLI<br />TTER</h1>
	<div class="card">
		<form id="calculator">
			<div class="input-group">
				<label for="bill">Bill</label>
				<input
					id="bill"
					class="field"
					type="number"
					min="1"
					step="0.1"
					placeholder="0"
					on:input={(e) => changeValue("bill", e.target.value)}
				/>
			</div>

			<div class="input-group">
				<label for="tips">Select Tip %</label>
				<ul class="tips" id="tips">
					{#each taxes as tax}
						<li class="tips-item">
							<button type="button" on:click={() => changeValue("tips", tax)}>
								{tax}%
							</button>
						</li>
					{/each}

					<li>
						<input
							id="custom"
							class="field"
							aria-label="Insira um valor customizado"
							type="number"
							min="1"
							step="0.5"
							placeholder="Custom"
							on:input={(e) => changeValue("tips", e.target.value)}
						/>
					</li>
				</ul>
			</div>

			<div class="input-group">
				<div class="input-header">
					<label for="peoples">Number of peoples</label>
					<small
						class={values.peoples === 0
							? "message-error"
							: "message-error hidden"}
					>
						Can`t be zero
					</small>
				</div>
				<input
					id="peoples"
					class={values.peoples === 0 ? "field error" : "field"}
					type="number"
					min="0"
					step="1"
					placeholder="0"
					on:input={(e) => changeValue("peoples", e.target.value)}
				/>
			</div>
		</form>

		<div class="display-wrap">
			<ul class="display">
				<li class="display-item">
					<h3 class="display-title">
						Tip Amount
						<small>/ person</small>
					</h3>
					<p id="amount" class="display-value">
						<img src="images/icon-dollar.svg" alt="" aria-hidden="true" />
						<span>{amount ? amount : "0.00"}</span>
					</p>
				</li>
				<li class="display-item">
					<h3 class="display-title">
						Total
						<small>/ person</small>
					</h3>
					<p id="total" class="display-value">
						<img src="images/icon-dollar.svg" alt="" aria-hidden="true" />
						<span>{total ? total : "0.00"}</span>
					</p>
				</li>
			</ul>

			<button
				type="reset"
				form="calculator"
				disabled={resetIsVisible ? "" : "disabled"}
				on:click={cleanForm}
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

	.input-group .field {
		text-align: right;
		font-size: 1.5rem;
		padding: 0 1rem;
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
		font-size: 1.25rem;
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

	.tips .field::placeholder {
		font-size: 1.25rem;
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
</style>
