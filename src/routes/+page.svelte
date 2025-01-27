<script>
	import Counter from '$lib/Counter.svelte';
</script>

<svelte:head>
	<title>Inicio</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>

	<Counter />
</section>
 <h1>Estimación Puntual</h1>
   <label for="datos">Introduce los datos (separados por comas):</label><br>
    <input type="text" id="datos"><br><br>

    <label for="confianza">Nivel de confianza (%):</label>
    <input type="number" id="confianza" value="95" min="1" max="99"><br><br>

    <button onclick="calcular()">Calcular</button>

    <div id="resultados">
        </div>

    <script>
        function calcular() {
            const datosStr = document.getElementById("datos").value;
            const confianza = parseFloat(document.getElementById("confianza").value) / 100;
            const datos = datosStr.split(",").map(Number).filter(Number.isFinite); // Convierte a números y filtra valores no válidos.

            if (datos.length === 0) {
                mostrarError("Por favor, introduce datos válidos.");
                return;
            }

            const n = datos.length;
            const media = datos.reduce((a, b) => a + b, 0) / n;
            const desviacionEstandar = Math.sqrt(datos.map(x => Math.pow(x - media, 2)).reduce((a, b) => a + b, 0) / (n - 1));

            // Cálculo del intervalo de confianza usando la distribución t de Student
            const gradosLibertad = n - 1;
            const t = jStat.studentt.inv(1 - (1 - confianza) / 2, gradosLibertad); // Usando jStat para la distribución t

            const margenError = t * (desviacionEstandar / Math.sqrt(n));
            const limiteInferior = media - margenError;
            const limiteSuperior = media + margenError;

            mostrarResultados(media, limiteInferior, limiteSuperior);
        }

        function mostrarResultados(media, limiteInferior, limiteSuperior) {
            const resultadosDiv = document.getElementById("resultados");
            resultadosDiv.innerHTML = `
                <p>Media muestral: ${media.toFixed(2)}</p>
                <p>Intervalo de confianza (${(parseFloat(document.getElementById("confianza").value)).toFixed(0)}%): [${limiteInferior.toFixed(2)}, ${limiteSuperior.toFixed(2)}]</p>
            `;
        }

        function mostrarError(mensaje) {
            const resultadosDiv = document.getElementById("resultados");
            resultadosDiv.innerHTML = `<p style="color: red;">${mensaje}</p>`;
        }
    </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jstat/1.9.5/jstat.min.js"></script>
   



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
