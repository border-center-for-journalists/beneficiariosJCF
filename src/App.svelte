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
<!-- <nav>
	<a href=''><img src='img/el-norte.png' alt='' /></a>
	<a href=''><img src='img/border-hub.jpg' alt='' style='width:200px'/></a>
</nav> -->
<main>
	<h1>¿Soy Beneficiario?</h1>
	<section class='container'>
		<p>Jóvenes Construyendo el Futuro es uno de los programas emblemáticos del Presiente Andrés Manuel López Obrador, sin embargo, ha sido utilizado para desviar recurso públicos engañando a beneficiarios para obtener sus datos  personales, inscribirlos al programa y robar su beca económica.</p>
 		<p>¿Quieres comprobar si fuiste parte de este engaño?</p>
		<p>Usa nuestra herramienta de búsqueda, ingresa tu nombre completo sin acentos para revisar si tus datos fueron utilizados.</p>
		<p>Si encuentras tu nombre da un click en la leyenda  <a target='_blank' href='https://wa.me/528119990007?text={message}'>“Soy yo, quiero denunciar”</a> y EL NORTE documentará el mal uso de tu información para evidenciar las anomalías del programa.</p>
		<!-- <p>El programa Jovenes construyendo un futuro a otorgado <strong> $40,529,994,316 pesos </strong> a <strong>2.1 millones</strong> de jovenes entre los años 2019 y 2020. Queremos transparentar el padron de beneficiarios para prevenir fraude y corrupcion en el programa.</p>

		<p>Busca tu nombre y si detectas alguna anomalia denuncia a la STPS o tambien a <a target='_blank' href='https://wa.me/528119990007?text={message}'>El Norte</a> con el fin de documentar posibles casos de operacion fraudulenta en el programa</p> -->
	</section>
	<form class='container'>
		<input type='text' placeholder="Busca tu nombre" on:keyup={({ target: { value } }) => debounce(value)} />
	</form>
	<section class='container'>
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
	</section>
</main>

<style>
	
	.container{
		max-width:840px;
		margin:0 auto;
		padding:0 20px;
	}
	h1{
		text-align: center;
		font-size:3em;
		margin:20px auto 0;
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