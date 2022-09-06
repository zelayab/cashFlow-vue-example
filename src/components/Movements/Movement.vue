<template>
    <div class="movement">
        <div class="content">
            <h4>{{ title }}</h4>
            <p>{{ description }}</p>
            <p>{{ date }}</p>
            <p>{{ type }}</p>

            {{ type }}
        </div>
        <div class="action">
            <img src="@/assets/trash-icon.svg" alt="delete" @click="remove(id)">
            <p :class="{ 'red': isNegative, 'green': !isNegative }">{{ amountCurrency }}</p>
        </div>
    </div>
</template>

<script setup>
import { toRefs, defineProps, computed, defineEmits } from 'vue';
// creamos currencyFormatter para dar formato a la moneda
const currencyFormatter = new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD',
});


// aqui definimos las props que vamos a usar en este componente
const props = defineProps({
    id: {
        type: Number,
    },
    title: {
        type: String,
    },
    description: {
        type: String,
    },
    amount: {
        type: Number,
    },
    type: {
        type: String,
    },
    date: {
        type: String,
    },
});

// aqui usamos toRefs para que se vuelva reactiva la variable
const { id, title, description, amount, type, date } = toRefs(props);

// aqui usamos currency para formatear el amount a currency
const amountCurrency = computed(() => currencyFormatter.format(amount.value));

// creamos una funcion para emitir el evento remove
// defineEmits retorna un objeto con la funcion emit que se usa para emitir el evento remove
const emit = defineEmits(['remove']);
const remove = () => {
    emit('remove', id.value);
};

const isNegative = computed(
    () => amount.value < 0
);// si es expense es negativo y si amount es menor a 0 es negativo;


</script>



<style scoped>
.movement {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    padding: 20px;
    background-color: #e6f9ff;
    border-radius: 8px;
    box-sizing: border-box;
}

.movement .content {
    width: 100%;
}

.movement .action {
    display: flex;
    justify-content: space-between;
    align-items: flex-end;
    flex-direction: column;
}

h4,
p {
    margin: 0;
    padding: 0;
}

h4 {
    margin-bottom: 8px;
}

.movement .action img {
    margin-bottom: 16px;
}

.red {
    color: red;
    font-weight: 600;
}

.green {
    color: green;
    font-weight: 600;
}
</style>