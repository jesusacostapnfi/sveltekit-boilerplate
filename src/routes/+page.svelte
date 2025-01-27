<script>
	import Counter from '$lib/Counter.svelte';
</script>

<svelte:head>
	<title>Inicio</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>

	<h2>
		try editing <strong>src/routes/index.svelte</strong>
	</h2>

	<Counter />
</section>
 <h1>Estimación Estadística</h1>
    <form method="post" action="">
        <label for="datos">Ingrese los datos separados por comas:</label>
        <input type="text" id="datos" name="datos">
        <input type="submit" value="Calcular">
    </form>

    <?php
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        // Obtener los datos del formulario
        $datos_str = $_POST['datos'];
        $datos_array = explode(',', $datos_str);
        $datos = array_map('floatval', $datos_array);

        // Calcular la media muestral
        $media = array_sum($datos) / count($datos);

        // Calcular el intervalo de confianza (ejemplo con nivel de confianza 95%)
        $z = 1.96; // Valor de z para un nivel de confianza del 95%
        $desviacion_estandar = stats_stddev($datos);
        $n = count($datos);
        $error_estandar = $desviacion_estandar / sqrt($n);
        $margen_error = $z * $error_estandar;
        $limite_inferior = $media - $margen_error;
        $limite_superior = $media + $margen_error;

        // Mostrar los resultados
        echo "<p>Media muestral: " . $media . "</p>";
        echo "<p>Intervalo de confianza (95%): [" . $limite_inferior . ", " . $limite_superior . "]</p>";
    }
    ?>
</body>



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

	
</style>
