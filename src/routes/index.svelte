<script>
	let promise = getRandomNumber();
    let data;
    $:water=0;
    $:wind=0;
    
	async function getRandomNumber() {
		setTimeout(() => getRandomNumber(), 15000)
        const res = await fetch(`http://localhost:8082/data`);
		const json = await res.json();

        let stat = json["status"];
        water=stat.water;
        wind=stat.wind;
        // console.log("water:",stat.water," - wind:",stat.wind);

		if (res.ok) {
			return stat;
		} else {
			throw new Error(res);
		}
	}


</script>

<svelte:head>
	<title>Danger-Flag Dashboard</title>
</svelte:head>

 <meta charset="UTF-8">
 
 <section>
	<h1>
		<div class="welcome">
			{#await promise}
				<p>...memuat</p>
			{:then data}
				<p>Kondisi air dan kecepatan angin terkini:</p>
				<h1>
					<table align="center">
						<tr>
							<td>🌊 <b>{water}</b> meter</td>
							<td style="padding-left: 1em;">
								{#if water <= 5}
								🟢 Aman
								{:else if water > 5 && water <9}
								🟡 Siaga
								{:else}
								🔴 Bahaya
								{/if}
							</td>
						</tr>
						<tr>
							<td><b>💨 {wind}</b> meter/detik</td>
							<td style="padding-left: 1em;">
								{#if wind <= 6}
								🟢 Aman
								{:else if wind > 6 && wind <16}
								🟡 Siaga
								{:else}
								🔴 Bahaya
								{/if}
							</td>
						</tr>
					</table>
				</h1>
			{:catch error}
				<p style="color: red">{error.message}</p>
			{/await}
		</div>
	</h1>


	
	

</section>	

 <style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}

	h1 {
		width: 100%;
	}

	.welcome {
		position: relative;
		width: 100%;
		height: 0;
		padding: 0 0 calc(100% * 495 / 2048) 0;
	}

	table td{
		justify-content: center;
		text-align: left;
	}

  </style>