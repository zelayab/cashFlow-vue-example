<template>
    <div>
        <header>
            <img @click="reload" src="@/assets/cashflow-icon.svg" alt="logo" />
            <h1>
                <span>Cash</span>Flow
            </h1>
        </header>
    </div>
</template>

<script>
export default {
    name: "Header",
    methods: {
        reload() {
            location.reload();
        }
    }
}
</script>
<style scoped>
header {
    display: flex;
    justify-content: space-around;
    align-items: center;
    width: 100vw;
}

img,
span {
    width: 48px;
}

h1 {
    font-size: 2rem;
    color: var(--brand-blue);
}

h1 span {
    color: var(--brand-green);
}
</style>
// Language: vue
// Path: cashflow\src\components\Header.vue
// Compare this snippet from cashflow\src\components\Layout.vue:
