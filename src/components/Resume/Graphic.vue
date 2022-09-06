<template>
    <div>
        <svg @touchstart="tap" @touchmove="tap" @touchend="untap" viewBox="0 0 300 200">
            <!-- line es para dibujar una linea -->
            <line x1="0" :y1="zero" x2="300" :y2="zero" stroke="#c4c4c4" stroke-width="2" />
            <!-- polyline es para dibujar una polilinea -->
            <polyline fill="none" stroke="#0689B0" stroke-width="2" :points="points" />

            <line v-show="showPointer" stroke="#07b500" stroke-width="2" :x1="pointer" y1="0" :x2="pointer" y2="200" />
        </svg>
        <p>Últimos 30 días</p>
    </div>
</template>

<script setup>
import { ref, toRefs, defineProps, computed, defineEmits, watch } from 'vue';

const props = defineProps({
    // configuramos las propiedades que recibira el componente y su tipo
    amounts: {
        type: Array,
        default: () => [],
    },
});

const { amounts } = toRefs(props);

const amountToPixels = (amount) => {
    const min = Math.min(...amounts.value);
    const max = Math.max(...amounts.value);
    const amountAbs = amount + Math.abs(min);
    const minmax = Math.abs(max) + Math.abs(min);
    return 200 - ((amountAbs * 100) / minmax) * 2;
};

const zero = computed(() => {
    return amountToPixels(0);
});

// funcion para dibujar el grafico en el eje x y y con los valores de amount que recibimos por props
const points = computed(() => {
    const total = amounts.value.length;
    return amounts.value.reduce((points, amount, i) => {
        const x = (300 / total) * (i + 1);
        const y = amountToPixels(amount);
        return `${points} ${x},${y}`;
    }, `0, ${amountToPixels(amounts.value.length ? amounts.value[0] : 0)}`);
});

// funcion para mostrar el puntero en el grafico cuando se hace tap    
const showPointer = ref(false);
// funcion para mostrar el puntero en el grafico cuando se hace tap
const pointer = ref(0);

// watch es como un observable. recibe la variable que escucha y la funcion que se ejecuta cuando cambia
watch(pointer, (value) => {
    const index = Math.ceil((value / (300 / amounts.value.length)));
    if (index < 0 || index > amounts.value.length) return;
    emit('select', {
        amount: amounts.value[index - 1],
    });
});

// funcion  para select el elemento en la pantalla
const emit = defineEmits(['select']);

// funcion para detectar el tap en el grafico y mostrar el valor del dia en el que se hizo
const tap = ({ target, touches }) => {
    showPointer.value = true;
    const elementWidth = target.getBoundingClientRect().width;
    const elementX = target.getBoundingClientRect().x;
    const touchX = touches[0].clientX;
    pointer.value = ((touchX - elementX) * 300) / elementWidth;
};
// funcion para detectar el untap en el grafico y ocultar el valor del dia en el que se hizo
const untap = () => {
    showPointer.value = false;
};

</script>

<style scoped>
svg {
    width: 100%;
}

p {
    text-align: center;
}
</style>