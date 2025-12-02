<template>
    <div class="box-component" 
    :class="isExtension ? 'extension' : 'header'">
        <slot />
    </div>
</template>

<script>
export default {
    name: 'BoxComponent',
    props: {
        isExtension: {
            type: Boolean,
            default: true,
        },
    },
};
</script>

<style scoped lang="scss"> 

.box-component {
    background-color: var(--secondary);
    min-width: 250px;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    padding: 10px 1rem;
    margin: 16px 5px;
}
.header {
    font-weight: bold;
    display: flex;
    justify-content: space-between; // left/right layout (left group vs toggle)
    gap: 0.5rem;
}
.extension {
    border: 2px solid var(--tertiary);
    max-width: 385px;
    width: 100%;
}
</style>