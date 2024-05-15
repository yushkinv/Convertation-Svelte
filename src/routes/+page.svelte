<!-- <h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p> -->

<script>   

    let url = 'https://open.er-api.com/v6/latest/USD';
    let ejson;    
    let ex1;
    let ex2;
    
    let selectOne;
    let selectTwo;
    
	let c;
	let f;

    let exchanges = [
            { rate: 1, currency: 'USD' },
            { rate: ex1, currency: 'RUB' },
            { rate: ex2, currency: 'EUR' },
        ]; 
    // Получение данных через API
    async function getJson() {
        let response = await fetch(url);
        ejson = await response.json();
               
        ex1 = ejson['rates']['RUB'];
        ex2 = ejson['rates']['EUR'];

        exchanges = [
            { rate: 1, currency: 'USD' },
            { rate: ex1, currency: 'RUB' },
            { rate: ex2, currency: 'EUR' },
        ];
        
        return exchanges
    }
    
    getJson()   

	function setBothFromC(value) {
		c = +value;
		f = +((selectTwo.rate * c)/selectOne.rate).toFixed(2);
	}

	function setBothFromF(value) {
		f = +value;
		c = +((selectOne.rate * f)/selectTwo.rate).toFixed(2);  
	}
</script>

<main>
    <h2>КАЛЬКУЛЯТОР ВАЛЮТ</h2>
    <div>
    <input value={c} on:input={(e) => setBothFromC(e.target.value)} type="number" placeholder="сумма для расчета" />
    <select bind:value={selectOne}>
        <option>Валюта</option>
        {#each exchanges as exchange}
            <option value="{exchange}">
                {exchange.currency}
            </option>  
        {/each}  
    </select>
    <span class="arrow left"></span>
    <span class="arrow right"></span>
    <input value={f} on:input={(e) => setBothFromF(e.target.value)} type="number" placeholder="сумма для расчета" />
    <select bind:value={selectTwo}>
        <option>Валюта</option>
        {#each exchanges as exchange}
            <option value="{exchange}">
                {exchange.currency}
            </option>  
        {/each}  
    </select>
    </div>
</main>

<style>
	input {        	
        max-width: 250px;
        height: 30px;
        font-size: 25px;
	}

    select,
    option {
        height: 36px;
        font-size: 25px;
    }

    .arrow {
        border: solid black;
        border-width: 0 3px 3px 0;
        display: inline-block;
        padding: 3px;
    }

    .right {
        transform: rotate(-45deg);
        -webkit-transform: rotate(-45deg);
    }

    .left {
        transform: rotate(135deg);
        -webkit-transform: rotate(135deg);
    }
</style>
