<template>
    <Layout>
        <template #header>
            <Header />
        </template>
        <template #resume>
            <Resume :label="label" :amount="amount" :total-amount="totalAmount" :total-label="totalLabel">
                <template #graphic v-show="isMobile < 768">
                    <Graphic :amounts="amounts" :dates="dates" @select="select" />
                </template>
                <template #action>
                    <Action @create="create" />
                </template>
            </Resume>
        </template>
        <template #movements>
            <Movements :movements="movements" @remove="remove" />
        </template>
    </Layout>
</template>

<script>
import Layout from "./Layout.vue";
import Header from "./Header.vue";
import Action from "@/components/Action.vue";
import Resume from "./Resume/Index.vue";
import Movements from "./Movements/Index.vue";
import Graphic from "./Resume/Graphic.vue";

// export default es para exportar el componente y poder usarlo en otros archivos como App.vue
export default {
    // components es para importar los componentes que se van a usar en este componente
    components: {
        Layout,
        Header,
        Resume,
        Movements,
        Action,
        Graphic,
    },
    // data es una funcion que retorna un objeto con las propiedades que queremos
    data() {
        // dentro de data podemos usar computed properties para retornar valores computados en el momento de la ejecucion
        return {
            label: "Current Balance",
            amount: null,
            totalAmount: "totalAmount",
            totalLabel: "Total Balance",
            movements: [],
        };
    },
    computed: {
        amounts() {
            const lastDays = this.movements
                .filter(m => {
                    const today = new Date();
                    const last30Days = today.setDate(today.getDate() - 30);
                    return m.date > last30Days;
                })
                .map(m => m.amount);
            return lastDays.map((m, i) => {
                const lastMovements = lastDays.slice(0, i + 1);

                return lastMovements.reduce((suma, movement) => suma + movement, 0);
            })
        },
        totalAmount() {
            return this.movements.reduce((suma, movement) => suma + movement.amount, 0);
        },
    },
    mounted() {
        // mounted es un hook que se ejecuta cuando el componente se monta en el DOM
        // en este caso se esta ejecutando la funcion getMovements para obtener los movimientos de la API y guardarlos en la variable movements
        // para poder usar la funcion getMovements tenemos que importarla en el archivo donde se esta usando (en este caso en Home.vue)
        const movements = localStorage.getItem('movements');
        if (Array.isArray(movements)) {
            // JSON.parse es para convertir un string en un objeto
            this.movements = JSON.parse(movements)?.map(m => {
                m.date = new Date(m.date).toLocaleString();
                return m;
            });
        }
    },
    methods: {
        // create es una funcion que recibe un objeto con las propiedades que queremos usar en el componente Action.vue y lo agrega al array movements
        // el metodo push agrega un elemento al final del array
        create(movement) {
            this.movements.push(movement);
            this.save();
        },
        //
        remove(id) {
            this.movements = this.movements.filter(m => m.id != id);
            this.save();
        },
        save() {
            // localStorage es una variable global que se puede usar en cualquier parte de la aplicacion para guardar datos
            // localStorage.setItem('key', 'value') guarda un valor en el localStorage
            // JSON.stringify es para convertir un objeto en un string
            localStorage.setItem("movements", JSON.stringify(this.movements));
        },
        select(el) {
            this.amount = el
        },
    }
}
</script>