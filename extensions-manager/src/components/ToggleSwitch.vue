<template>
    <div class="toggle-switch">
        <div class="switch-bg" :class="dataActive ? 'active' : 'inactive'" @click="toggleActive">
            <!-- simulate toggle switch moving left/right -->
            <div v-if="!dataActive" class="toggle-off"> <!-- left --></div>
            <div v-else class="toggle-on"> <!-- right --></div>
        </div>
    </div>
</template>

<script>
export default {
    name: "ToggleSwitch",
    props: {
        isActive: {
            type: Boolean,
        }
    },
    data() {
        return {
            dataActive: this.isActive,
        }
    },
    watch: {
        isActive(newVal) {
            this.dataActive = newVal;
        }
    },
    methods: {
        toggleActive() {
            this.dataActive = !this.dataActive;
            this.$emit('toggleActive');
        }
    }
}
</script>

<style>
.switch-bg {
    height: 25px;
    width: 50px;
    border-radius: 500px;
    display: inline-flex;
    &.active {
        background-color: var(--medium-accent);
    }
    &.inactive {
        background-color: var(--tertiary);
    }
    &:hover {
        cursor: pointer;
        border: 1px solid var(--medium-accent);
    }
}
.toggle-on, .toggle-off {
    height: 21px;
    width: 21px;
    border-radius: 50%;
    background-color: white;
    margin: 2px;
}
.toggle-off {
    margin-right: auto;
}
.toggle-on {
    margin-left: auto;
}
</style>