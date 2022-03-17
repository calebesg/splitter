<script>

	const values = {
		bill: 0,
		tips: 0,
		peoples: 0,
	}

	let amount = 0;
	let total = 0;

	let resetIsVisible = false;

  const taxes = [5, 10, 15, 25, 50];

	function cleanForm() {
		values.bill = 0;
		values.tips = 0;
		values.peoples = 0;

		total = 0;
		amount = 0;

		resetIsVisible = false;
	}

	function changeValue(field, value) {
		values[field] = value;
		calculate();
	}

	function calculate() {
		const areFilled = validateFields();

		if (areFilled === false) {
			return false;
		}

		const { bill, tips, peoples } = values;

		total = bill * (tips / 100 + 1);
		amount = total / peoples;

		resetIsVisible = true;
	}

	function validateFields() {
		const { bill, tips, peoples } = values;

		if (peoples === 0 || bill === 0 || tips === 0) {
			return false;
		}

		return true;
	}

</script>

<main>
	<form>
		<label for="bill">Bill</label>
		<input
			id="bill"
			type="number"
			value={values.bill}
			min="1"
			step="0.1"
			on:input={ e => changeValue('bill', e.target.value) }
		/>
	
		<label for="tips">Tips %</label>
		<ul class="tips" id="tips">
			{#each taxes as tax}
				<li class="tips-item">
					<button 
						type="button" 
						on:click={ () => changeValue('tips',tax) }
					>
						{tax}%
					</button>
				</li>
			{/each}
			
			<li>
				<input 
					id="custom"
					aria-label="Insira um valor customizado"
					type="number"
					min="1"
					step="0.5"
					value={ values.tips === 0 && '' }
					placeholder="Custom"
					on:input={ e => changeValue('tips', e.target.value) }
				/>
			</li>
		</ul>
	
		<label for="peoples">Number of peoples</label>
		<input
			id="peoples"
			type="number"
			value={values.peoples}
			min="1"
			step="1"
			on:input={ e => changeValue('peoples', e.target.value) }
		/>	
	</form>

	<div class="display">
		<ul>
			<li>
				<p>
					Tip Amount 
					<small>/ person</small>
				</p>
				<h2 id="amount">
					<img src="./" alt="">
					<span>{ amount ? amount : '0.00'}</span>
				</h2>
			</li>
			<li>
				<p>
					Total 
					<small>/ person</small>
				</p>
				<h2 id="total">
					<img src="./" alt="">
					<span>{ total ? total : '0.00'}</span>
				</h2>
			</li>
		</ul>

		<button
			type="reset"
			disabled={ resetIsVisible ? '' : 'disabled' }
			on:click={ cleanForm }
			>RESET</button>
	</div>
</main>

<style>
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	form ul {
    list-style: none;
	}
  
  .tips {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    gap: 1rem;
  }
  
	form input {
    text-align: right;
	}
</style>