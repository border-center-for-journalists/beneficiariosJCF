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
	<h1><a target='_blank' href='https://soybeneficiario.borderhub.org'>¿Soy Beneficiario?</a></h1>
	<section class='container'>
		<p>Jóvenes Construyendo el Futuro es uno de los programas emblemáticos del Presidente Andrés Manuel López Obrador, sin embargo, ha sido utilizado para desviar recurso públicos engañando a beneficiarios para obtener sus datos  personales, inscribirlos al programa y robar su beca económica.</p>
 		<p>¿Quieres comprobar si fuiste parte de este engaño?</p>
		<p>Usa nuestra herramienta de búsqueda, ingresa tu nombre completo sin acentos para revisar si tus datos fueron utilizados.</p>
		<p>Si encuentras tu nombre da un click en la leyenda  <a target='_blank' href='https://wa.me/528119990007?text={message}'>“Soy yo, quiero denunciar”</a> y EL NORTE documentará el mal uso de tu información para evidenciar las anomalías del programa.</p>
		<!-- <p>El programa Jovenes construyendo un futuro a otorgado <strong> $40,529,994,316 pesos </strong> a <strong>2.1 millones</strong> de jovenes entre los años 2019 y 2020. Queremos transparentar el padron de beneficiarios para prevenir fraude y corrupcion en el programa.</p>

		<p>Busca tu nombre y si detectas alguna anomalia denuncia a la STPS o tambien a <a target='_blank' href='https://wa.me/528119990007?text={message}'>El Norte</a> con el fin de documentar posibles casos de operacion fraudulenta en el programa</p> -->
	</section>
	<form class='container'>
		<img src="..\img\search-solid.svg" alt="Búsqueda" width = "30px">
		<input type='text' placeholder="Busca tu nombre" on:keyup={({ target: { value } }) => debounce(value)} />
		<img src="..\img\times-solid.svg" alt="Cerrar" width = "15px">
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
						<span class='cta whatsapp'>¡Soy yo! <a target='_blank' class="button" href='https://wa.me/528119990007?text={makeMessage(beneficiario)}'> Denunciar<img src="..\img\whatsapp-brands.svg" alt="Whatsapp" width="30px"/></a></span>
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
	
	section.container{
		max-width:840px;
		margin:0 auto;
		padding:0 20px;
	}
	h1{
		text-align: center;
		font-size:2.7em;
		margin:20px auto 0;
		border-bottom:1px solid #E7E7E7;
		max-width: 480px;
		margin-bottom:30px;
		padding-bottom:20px;
	}
	h1 a{
		color:black;
	}
	h1 a:hover{
		text-decoration: none;
	}
	ul{
		list-style-type:none;
		padding:0;
	}
	li{
		background-color: black;
		color: white;
		letter-spacing: 1px;
		font-weight: lighter;
		

		display: flex;
		flex-direction: column;
		margin-bottom:10px;
		border-bottom:1px solid #F3E44B;
		padding:2em;
	}
	li span:first-child{
		font-size:25px;
	}
	li span:nth-child(2){
		margin-top: 10px;
		margin-bottom: 10px;
		padding-bottom: 10px;
		font-size: 15px;
		border-bottom: 2px solid #F3E44B;
	}
	li span:nth-child(3){
		font-size: 25px;
		margin-bottom: 20px;
	}
	li span:nth-child(4){
		font-size: 18px;
		color: #F3E44B;
		font-weight: bold;
	}
	a{
		color: #F3E44B;
	}
	p {
		font-weight: bold;
		text-align: center;
	}
	img{
		vertical-align: middle;
		margin-left: 10px;
		margin-right: 20px;
        margin-left: 10px;
        vertical-align: middle;
	}


	/*Form*/
	input{
        border: none;
        border-bottom: 1px solid black;
        background-color: #F3E44B;
        margin-top: 2em;
        margin-bottom: 2em;
        font-weight: bold;
        color: black;
        width: 500px;
    }
    form.container{
        background-color: #F3E44B;
        display: flex;
        align-items: center;
        justify-content: center;
        margin-bottom: 30px;

		max-width:none !important;
		margin:0;
		padding:0;
    }
	a.button{
		appearance: button;
		background-color: #F3E44B;
		color: #000000;
		padding-top: 5px;
		padding-bottom: 5px;
		padding-right: 18x;
		padding-left: 20px;
		text-decoration: none;
		margin-left: 20px;
	}
	a.button:hover{
		background-color: #ffea00;
	}
</style>