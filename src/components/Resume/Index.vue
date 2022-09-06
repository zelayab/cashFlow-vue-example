<template>
    <main>
        <h5>{{ totalLabel }} : {{ totalAmount }}</h5>
        <p>
            {{ labelVisual }}
        </p>
        <h1 :class="{ 'red': isNegative, 'green': !isNegative }">
            {{ amountCurrency }}
        </h1>
        <div class="graphic">
            <slot name="graphic"></slot>
        </div>
        <div class="action">
            <slot name="action"></slot>
        </div>
    </main>
</template>

<script>

export default {
    props: {
        label: {
            type: String,
        },
        totalLabel: {
            type: String,
        },
        totalAmount: {
            type: Number,
        },
        amount: {
            type: Number,
            default: null,
        },
    },
    computed: {
        /* aqui como usamos options API necesitamos el uso de THIS. Aqui si no es null muestra amount, caso contrario total Amount */
        amountVisual() {
            return this.amount != null ? this.amount : this.totalAmount;
        },
        /* labelVisual tiene la misma opcion que amountVisual si retorna null */
        labelVisual() {
            return this.label != null ? this.label : this.totalLabel;
        },
        /* funcion computada para retornar numero a currency */
        /*  Intl.NumberFormat es de JS vanilla para dar formato a la moneda */
        amountCurrency() {
            return new Intl.NumberFormat('en-US', {
                style: 'currency',
                currency: 'USD',
            }).format(this.amountVisual);
        },
        /* funcion computada para saber si es negativo */
        isNegative() {
            return this.amountVisual < 0;
        },

    },
};


</script>

<style scoped>
main {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    width: 100%;
}

h1,
p {
    margin: 0;
    text-align: center;
}

h1 {
    margin-top: 14px;
}

.graphic {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    padding: 48px 24px;
    box-sizing: border-box;
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