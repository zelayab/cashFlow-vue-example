<template>
    <div class="movements">
        <h2 class="title">History</h2>
        <div class="content">
            <Movement v-for="{ id, title, description, amount, date, type } in movements" :key="id" :id="id"
                :title="title" :description="description" :type="movementType" :amount="amount" :date="date"
                @remove="remove" />
        </div>
    </div>
</template>
// aqui usamos Composition API, no es necesario usar del export default ni el setup, directamente se ejecuta aqui mismo
<script setup>
import { toRefs, defineProps, defineEmits } from 'vue';
import Movement from './Movement.vue';

// aqui definimos las props que vamos a usar en este componente
// con defineProps podemos definir el tipo de dato que va a recibir cada prop y si es requerida o no (required)
const props = defineProps({
    movements: {
        type: Array,
        // aqui tiene que ser una funcion que retorna un array de objetos con las propiedades que queremos que tenga para que tenga reactividad
        default: () => [],
    },
});
// para poder hacerlo reactivo tenemos que usar toRef para que se vuelva reactiva la variable movements
const { movements } = toRefs(props);

// aqui usamos defineEmits para definir los eventos que vamos a emitir
const emit = defineEmits(['remove']);
// creamos una funcion para emitir el evento remove con un handler
const remove = (id) => {
    // aqui emitimos el evento remove con el id del movimiento que queremos eliminar
    emit('remove', id);
    // el handler esta en el componente padre
};
</script>

<style scoped>
.movements {
    max-height: 100%;
    padding: 10px 8px;
    margin: 20px;
}

.title {
    margin: 8px 16px 24px 16px;
    color: var(--brand-blue);
}

.content {
    max-height: 68vh;
    display: flex;
    flex-direction: column;
    gap: 8px;
    overflow-y: scroll;
}
</style>