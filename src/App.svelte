<script>
let testi = 0
let slimie = 0
let ipatsvars_value
let filter = ""
const iedzivotaji = 1919968

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
	const atlicis = input => iedzivotaji - input
</script>

{#await getData()}
  loading...
{:then records}
<main>
<section>
	<img src="https://www.crwflags.com/fotw/images/l/lv.gif" alt="Latvijas karogs"/>
	<p>Populācija: {iedzivotaji}</p>
	<p>Kopējais testu skaits: {testi}</p>
	<p>Kopējais inficēto skaits: {slimie}</p>
	<p>Atlicis notestēt: {atlicis(testi)}</p>
</section>

<section>
<table>
	<thead>
	<tr>
		<th>Datums</th>
		<th>Testi</th>
		<th>Inficētie</th>
		<th><input type=number bind:value={ipatsvars_value} placeholder="% īpatsvars  (filtrs)"/></th>
	</tr>
	</thead>
	<tbody>
		{#each numberFilter(records, 'Ipatsvars', ipatsvars_value | 0) as record}
			<tr>
				<td>{record.Datums.split("T00")[0]}</td>
				<td>{record.TestuSkaits}</td>
				<td>{record.ApstiprinataCOVID19InfekcijaSkaits}</td>
				<td class="active">{record.Ipatsvars}</td>
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
	img{
		min-width:100px;
		width:100%;
		max-height:100px;
		object-fit:fill;
	}
	main{
		display:grid;
		grid-auto-flow: column;
		grid-template-columns: 250px 1fr;
		gap:20px;
		max-width:960px;
		margin: 0 auto;
	}
	input{
		width:100%;
/* 		max-width:150px; */
	}
	table{
/* 		width:100%; */
		border-collapse: collapse;
/*     margin: 25px 0; */
    font-size: 0.9em;
    font-family: sans-serif;
/*     min-width: 300px; */
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
</style>
