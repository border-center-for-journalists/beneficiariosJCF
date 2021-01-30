<script>
	import fetchNode from './helpers/fetchNode.js';
	let timer;
	let query = '';
	let message = 'Hola, aparezco en el padron de beneficiarios de Jovenes construyendo futuro pero quiero reportar una anomalía.';
	$: params = {nombreCompleto:{contains:query.toUpperCase()}};
	$: beneficiarioPromise = fetchNode('beneficiario', {where:JSON.stringify(params),sort:'importe DESC'});

	const debounce = v => {
		clearTimeout(timer);
		timer = setTimeout(() => {
			query = v;
		}, 600);
	};

	const formatCurrency = num => {
		return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' }).format(num)
	};

	const makeMessage = ben => 'Hola, aparezco en el padron de beneficiarios de Jovenes construyendo futuro pero quiero reportar una anomalía. Mi nombre es '+ben.nombreCompleto+'.';

</script>

<main>
	<section>
		<h1>¿Soy Beneficiario?</h1>
		<p>El programa Jovenes construyendo un futuro a otorgado <strong> $ 40,529,994,316 pesos </strong> a <strong>2.1 millones</strong> de jovenes entre los años 2019 y 2020. Queremos transparentar el padron de beneficiarios para prevenir fraude y corrupcion en el programa.</p>

		<p>Busca tu nombre y si detectas alguna anomalia denuncia a la STPS o tambien a <a target='_blank' href='https://wa.me/528119990007?text={message}'>El Norte</a> con el fin de documentar posibles casos de operacion fraudulenta en el programa</p>
	</section>
	<form>
		<input type='text' placeholder="Busca tu nombre" on:keyup={({ target: { value } }) => debounce(value)} />
	</form>
	{#await beneficiarioPromise}
		<p>buscando...</p>
	{:then beneficiarios}
	<ul>
		{#if beneficiarios.length}
			{#each beneficiarios as beneficiario}
				<li>
					<span>{beneficiario.nombreCompleto}</span>
					<span>{beneficiario.municipio.nombreMunicipio}, {beneficiario.municipio.nombreEntidad}</span>
					<span>{formatCurrency(beneficiario.importe)}</span>
					<span class='cta whatsapp'><a target='_blank' href='https://wa.me/528119990007?text={makeMessage(beneficiario)}'> Soy yo, quiero denunciar (whatsapp)</a></span>
				</li>
			{/each}
		{:else}
			<p>Sin resultados</p>
		{/if}
	</ul>
	{/await}
</main>

<style>
	main{
		display: flex;
		flex-direction: column;
		max-width:840px;
		margin:0 auto;
		padding:0 20px;
	}
	section h1{
		text-align: center;
		font-size:3em;
	}
	ul{
		list-style-type:none;
		padding:0;
	}
	li{
		display: flex;
		flex-direction: column;
		margin-bottom:8px;
		border-bottom:1px solid pink;
		padding:10px 0;
	}
	li span:first-child{
		font-size:16px;
		font-weight: bold;
	}
	strong{font-size:1.3em;}
</style>