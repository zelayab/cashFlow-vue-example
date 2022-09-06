<template>
    <button @click="showModal = true">
        Add Movement
    </button>
    <!-- con teleport podemos mover el modal a cualquier parte de la pagina, en este caso a #app que es el componente raiz -->
    <teleport to="#app">
        <!-- aqui esta modal, que se muestra si la variable showModal es true-->
        <!-- y aqui escuchamos el evento close del modal por lo tanto pasamos a false showModal -->
        <Modal v-show="showModal" @close="showModal = false">
            <form @submit.prevent="submit">
                <div class="field">
                    <label for="title">Title</label>
                    <input type="text" v-model="title" placeholder="Title">
                </div>
                <div class="field">
                    <label for="description">Description</label>
                    <textarea v-model="description" placeholder="Description"></textarea>
                </div>
                <div class="field">
                    <label for="amount">Amount</label>
                    <input type="number" v-model="amount" placeholder="Amount">
                </div>
                <div class="field">
                    <label for="type">Type</label>
                    <select v-model="type">
                        <option value="income">Income</option>
                        <option value="expense">Expense</option>
                    </select>
                </div>
                <div class="field">
                    <label for="date">Date</label>
                    <input type="date" v-model="date" placeholder="Date">
                </div>
                <div class="field action">
                    <button>Add new movement</button>
                </div>
            </form>
        </Modal>
    </teleport>
</template>

<script setup>
import { ref, defineEmits } from 'vue';
import Modal from './Modal.vue';

// creamos una variable para mostrar el modal o no
const showModal = ref(false);
const title = ref('');
const description = ref('');
const amount = ref(0);
const date = ref('');
const type = ref('income');


const emit = defineEmits(['create']);

// creamos una funcion para enviar el formulario
const submit = () => {
    showModal.value = !showModal.value;
    emit('create', {
        title: title.value,
        description: description.value,
        amount: type.value === 'income' ? amount.value : -amount.value,
        date: new Date().toLocaleString(),
        id: new Date(),
        type: type.value,
    });
    title.value = "";
    description.value = "";
    amount.value = 0;
    type.value = "Income";
    date.value = "";
    console.log(type.value)


};
</script>

<style scoped>
button {
    color: white;
    font-size: 1.25rem;
    background-color: var(--brand-blue);
    border: none;
    width: 100%;
    padding: 24px 60px;
    border-radius: 60px;
    box-sizing: border-box;
}

form {
    font-size: 1.24rem;
    width: 100%;
    background: #fff;
}

form .action {
    padding: 0 24px;
}

.field {
    display: flex;
    justify-content: space-between;
    flex-direction: column;
    padding: 16px 24px;
}

label {
    margin-bottom: 8px;
}

input,
textarea,
select {
    font-size: 1.24rem;
    border: 2px solid var(--brand-blue);
    border-radius: 8px;
    padding: 8px;
}

input[type="number"] {
    text-align: right;
}

.radio-label {
    display: flex;
    align-items: center;
    margin-top: 8px;
}

.radio-label span {
    margin-top: 4px;
    margin-left: 8px;
}

input[type="radio"] {
    appearance: none;
    width: 1.24rem;
    height: 1.24rem;
    color: var(--brand-blue);
    border: 2px solid var(--brand-blue);
    border-radius: 50%;
}

input[type="radio"]:checked {
    background-color: var(--brand-blue);
}
</style>