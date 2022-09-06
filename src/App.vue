<template>
    <!-- Suspense es para que cargue los slots: default es el componente que queremos mostrar cuando todo cargue -->
    <!-- el fallback vamos a cargar mientras no haya un Home -->
    <Suspense>
        <template #default>
            <Home />
        </template>
        <template #fallback>
            <SplashScreen />
        </template>
    </Suspense>
</template>

<script>
    import SplashScreen from "@/components/SplashScreen.vue";
    import { defineAsyncComponent } from "vue";

    export default {
        name: "App",
        components: {
        SplashScreen,
        // defineasynccomponent is used to load components asynchronously and is used to load the Home component
        // usamos una promesa con un resolve para que se ejecute cuando se resuelva la promesa y se cargue el componente Home
        // asi lo carga asincronamente.
        Home: defineAsyncComponent(() =>
            new Promise((resolve) => {
            setTimeout(() => {
                resolve(import("./components/Home.vue"));
            }, 3000);
            })
            ),
        },
    };
</script>

<style>
html,
body,
.app {
    min-height: 100vh;
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
}

* {
    --brand-green: #04b500;
    --brand-blue: #0689b0;
}
</style>
