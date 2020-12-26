<script>
	const iedzivotaji = 1907675
	let testi = 0
	let slimie = 0
	let filter = "" 
	let ipatsvars_value
	let papildus = false
	const atlicis = input => iedzivotaji - input
	const percentage = (partialValue, totalValue) => ((100 * partialValue) / totalValue).toFixed(2)
	const numberFilter = (collection, key, value) => collection.filter(entry => entry[key] > value).reverse()
	
	async function getData() {
		const res = await fetch('https://data.gov.lv/dati/lv/api/3/action/datastore_search?resource_id=d499d2f0-b1ea-4ba2-9600-2c701b03bd4a&limit=500');
		const data = await res.json()
		const result = data.result.records
		result.forEach(entry => {
			testi += entry.TestuSkaits
			slimie += entry.ApstiprinataCOVID19InfekcijaSkaits
		})
		return result
	}
	
	</script>
	
	{#await getData()}
	  loading...
	{:then records}
	<main>
	
		<header> 
				<article>
					<h1>SARS-CoV-2 statistika Latvijā</h1>
					<ul>
						<li>Kopējā populācija: {iedzivotaji} </li>
						<li>Kopējais inficēto skaits: {slimie} - %{percentage(slimie, iedzivotaji)}</li>
						<li>Kopējais testu skaits: {testi} - %{percentage(testi, iedzivotaji)}</li>
						<li>Atlicis notestēt: {atlicis(testi)} - %{percentage(atlicis(testi), iedzivotaji)}</li>
					</ul>
					<h3>Datu avoti</h3>
				<ul>
					<li><a href="https://data.gov.lv/dati/lv/dataset/covid-19/resource/d499d2f0-b1ea-4ba2-9600-2c701b03bd4a">Covid-19 Izmeklējumi</a></li>
					<li><a href="http://data1.csb.gov.lv/pxweb/lv/iedz/iedz__iedzskaits__ikgad/ISG010.px">Iedzīvotāju skaits</a></li>
				</ul>
				</article>
				<article>
					<img src="https://www.crwflags.com/fotw/images/l/lv.gif" alt="Latvijas karogs"/>
				</article>
			
			</header>

	<section>
		<p><small>(Tiek uzskatīts, ka slimība sabiedrībā izplatās nekontrolēti, ja pozitīvo testu īpatsvars pārsniedz 4%)</small></p>
		<article>
		
			<div>
				Izvērsti dati<input type=checkbox bind:checked={papildus}>
			</div>	
			<div>
				<input id="ipatsvars" type=number bind:value={ipatsvars_value} placeholder="īpatsvara % filtrs"/>
			</div>
			
			
		</article>
			
			
	<table>

		<thead>
		<tr>
			<th>Datums</th>
			<th>Testi</th>
			<th>Inficētie</th>
			<th>Miruši</th>
			<th>Īpatsvars</th>
			{#if papildus}
				<th>Atves.</th>
				<th>0-9</th>
				<th>10-19</th>
				<th>20-29</th>
				<th>30-39</th>
				<th>40-49</th>
				<th>50-59</th>
				<th>60-69</th>
				<th>70+</th>
				<th>70-79</th>
				<th>80+</th>
			{/if}
		</tr>
		</thead>
		<tbody>
			{#each numberFilter(records, 'Ipatsvars', ipatsvars_value | 0) as record}
				<tr>
					<td>{record.Datums.split("T00")[0]}</td>
					<td>{record.TestuSkaits}</td>
					<td>{record.ApstiprinataCOVID19InfekcijaSkaits}</td>
					<td>{record.MirusoPersonuSkaits}</td>
					<td class="active">{record.Ipatsvars}</td>
					{#if papildus}
						<td>{record.IzarstetoPacientuSkaits}</td>
						<td>{record['ApstiprinatiVecGr_0-9Gadi']}</td>
						<td>{record['ApstiprinatiVecGr_10-19Gadi']}</td>
						<td>{record['ApstiprinatiVecGr_20-29Gadi']}</td>
						<td>{record['ApstiprinatiVecGr_30-39Gadi']}</td>
						<td>{record['ApstiprinatiVecGr_40-49Gadi']}</td>
						<td>{record['ApstiprinatiVecGr_50-59Gadi']}</td>
						<td>{record['ApstiprinatiVecGr_60-69Gadi']}</td>
						<td>{record['ApstiprinatiVecGr_70GadiUnVairak']}</td>
						<td>{record['ApstiprinatiVecGr_70-79Gadi']}</td>
						<td>{record['ApstiprinatiVecGr_80GadiUnVairak']}</td>
					{/if}
					
				</tr>
			{/each}
		</tbody>
	</table>
	</section>
	</main>
	{:catch e}
		error {e}
	{/await}
	
	
	
	<style>
		.grid{
			gap:10px;
			display:grid;
			grid-template-columns: repeat(auto-fit, minmax(240px, 1fr))
		}
		/* input{
			margin:2px;
			padding:10px;
		} */
		img{
			min-width:100px;
			width:150px;
			max-height:100px;
			object-fit:fill;
		}
		main{

			/* display:grid; */
			grid-auto-flow: column;
			grid-template-columns: 300px 1fr;
			gap:20px;
			/* max-width:960px; */
			/* max-width: 1200px;; */
			margin: 0 auto;
		}

		table{
			margin: 0 auto;
			width:100%;
			border-collapse: collapse;
	    /* margin: 0px 25px 25px 0px; */
		font-size: 0.9em;
		font-family: sans-serif;
	/*     min-width: 300px; */
		box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
		max-width:1200px;
		}
		thead tr {
		background-color: #990000;
		color: #ffffff;
		text-align: left;
	}
		th,td {
		padding: 12px 15px;
	}
		
	tbody tr {
		border-bottom: thin solid #dddddd;
	}
	
	tbody tr:nth-of-type(even) {
		background-color: #f3f3f3;
	}
	
	tbody tr:last-of-type {
		border-bottom: 2px solid #990000;
	}
		
	.active {
		font-weight: bold;
		color: #990000;
	} 
	main{
		max-width:1200px;
		padding-bottom: 20px;
	}

	header{
		display: flex;
		justify-content: space-between;
	}
	header article img{
		margin-block-start: 1em;
    	margin-block-end: 1em;
	} 
	section article{
		display:flex;
		justify-content: space-between;
		
	}
	section article div{
		align-self: flex-end;
	}
	input {
		margin-left: 5px;
	}
	</style>
	