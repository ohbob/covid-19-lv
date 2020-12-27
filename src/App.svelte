<script>
	const iedzivotaji = 1907675
	let testi = 0
	let slimie = 0
	let mirusi = 0
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
			mirusi += entry.MirusoPersonuSkaits
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
					<img src="https://www.crwflags.com/fotw/images/l/lv.gif" alt="Latvijas karogs"/>
					<h1>SARS-CoV-2 statistika Latvijā</h1>
					<table>
						<tbody>
							<tr>
								<td><b>Populācija</b></td>
								<td><b>Inficētie</b></td>
								<td><b>Veiktie testi</b></td>
								<td><b>Nepārbaudīti</b></td>
								<td><b>Miruši</b></td>
							</tr>
								<tr>
									<td>{iedzivotaji}</td>
									<td>{slimie}</td>
									<td>{atlicis(testi)}</td>
									<td>{testi}</td>
									<td>{mirusi}</td>
								</tr>
								<tr>
									<td></td>
									<td>{percentage(slimie, iedzivotaji)}%</td>
									<td>{percentage(testi, iedzivotaji)}%</td>
									<td>{percentage(atlicis(testi), iedzivotaji)}%</td>
									<td>{percentage(mirusi, iedzivotaji)}%</td>
								</tr>
						</tbody>
					</table>
					<p><small>Tiek uzskatīts, ka slimība sabiedrībā izplatās nekontrolēti, ja pozitīvo testu īpatsvars pārsniedz 4% robežu.</small></p>			
				</article>
				<article style="text-align:end;">
				
					<h3>Datu avoti</h3>
				<ul>
					<li><a href="https://data.gov.lv/dati/lv/dataset/covid-19/resource/d499d2f0-b1ea-4ba2-9600-2c701b03bd4a">Covid-19 Izmeklējumi</a></li>
					<li><a href="https://data1.csb.gov.lv/pxweb/lv/iedz/iedz__iedzskaits__ikgad/ISG010.px">Iedzīvotāju skaits</a></li>
				</ul>
					
				</article>
			
			</header>

	<section>
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
		ul{
			list-style-type: none;
			margin: 0;
			padding:0;
		}

		img{
			width:100%;
			
			max-width:150px;
			max-height:100px;
			object-fit:fit;
		}
		main{
			margin: 0 auto;
			max-width:1200px;
		}

		table{
		margin: 0 auto;
		width:100%;
		border-collapse: collapse;
		font-size: 0.9em;
		font-family: sans-serif;
		box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
		
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
		display: grid;
  		grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
		gap:50px;
	}

	header article{
		/* flex-wrap: wrap; */
	}
	header article img{
		margin-block-start: 1em;
    	margin-block-end: 1em;
	} 
	section article{
		margin-top:50px;
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
	