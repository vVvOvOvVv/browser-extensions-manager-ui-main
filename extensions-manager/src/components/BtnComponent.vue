<template>
    <div class="btn-component" @click="clickFunction">
        {{ btnText }}
    </div>
</template>

<script>
export default {
    name: "BtnComponent",
    props: {
        btnFunction: {
            type: String,
            default: "remove",
        }
    },
    data() {
        return {
            btnText: "",
            isSelected: false,
        }
    },
    mounted() {
        switch (this.btnFunction) {
            case "remove":
                this.btnText = "Remove";
                break;
            case "sort-all":
                this.btnText = "All";
                break;
            case "sort-active":
                this.btnText = "Active";
                break;
            case "sort-inactive":
                this.btnText = "Inactive";
                break;
            default:
                this.btnText = "Button";
        }
    },
    methods: {
        clickFunction() {
            switch (this.btnFunction) {
                case "remove":
                    this.$emit('removeExtension');
                    break;
                case "sort-all":
                    this.$emit('sortAll');
                    break;
                case "sort-active":
                    this.$emit('sortActive');
                    break;
                case "sort-inactive":
                    this.$emit('sortInactive');
                    break;
                default:
                    console.error("BtnComponent: Unknown button function");
            }
        }
    }
}
</script>

<style>
    .btn-component {
        background-color: var(--secondary);
        border-radius: 500px;
        border: 2px solid var(--quaternary);
        padding: 5px 10px;
        cursor: pointer;
        &:hover {
            border: 2px solid var(--medium-accent);
            background-color: var(--tertiary);
        }
        
    }
</style>