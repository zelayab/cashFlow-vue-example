// Layout tiene la logica de la pagina, Header tiene la logica del header, y Home tiene la logica de la pagina Home
// Layout permite que Header y Home sean reutilizables en otras paginas
// Language: vue

<template>
	<div class="header">
		<!-- el slot permite definir y acomodar los elementos en la pantalla-->
		<slot name="header"></slot>
	</div>
	<!-- Resume es el componente donde se muestra el grafico y recibe dos props, uno para el label y otro para el monto -->
	<div class="resume">
		<slot name="resume"></slot>
	</div>
	<!-- movements muestra el historial y tiene un grapper que funciona como una especie de modal -->
	<!-- Este es un componente que se abre y se cierra, se cambia de verdadero a falso -->
	<div class="movements">
		<div class="head" @click="showMovements = !showMovements">
			<div class="grip"></div>
		</div>
		<div class="body" v-show="showMovements">
			<div class="title">
				<h2>Movements</h2>
			</div>
			<div class="content">
				<slot name="movements"></slot>
			</div>
		</div>
	</div>
</template>

// con script setup podemos definir los props de la siguiente manera y no es necesario exportarlos como en el caso de los componentes normales
// aqui usamos composition API para definir los props y los metodos de la siguiente manera
<script setup>
import { ref } from 'vue';
const showMovements = ref(false);
</script>

<style scoped>
.header,
.resume,
.movements {
	display: flex;
	justify-content: space-around;
	align-items: center;
	padding: 14px 0;
	box-sizing: border-box;
}

.header {
	position: fixed;
	width: 100vw;
}

.resume {
	min-height: 100vh;
}

.movements {
	z-index: 1;
	position: absolute;
	flex-direction: column;
	bottom: 0;
	width: 100vw;
	background-color: white;
	box-shadow: 0 -8px 16px #e5e5e5;
	border-radius: 24px;
}

.movements .head {
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 24px;
	width: 100%;
	box-sizing: border-box;
}

.movements .body {
	height: 75vh;
	width: 100%;
}

.movements .head .grip {
	width: 120px;
	height: 8px;
	background-color: #e5e5e5;
	border-radius: 4px;
}
</style>