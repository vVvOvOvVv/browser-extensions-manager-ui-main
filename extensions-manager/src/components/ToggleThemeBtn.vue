<template>
    <div class="toggle-theme-btn" @click="toggleMode">
        <img v-if="isDark" src="../assets/images/icon-sun.svg" alt="Switch to light mode"/>
        <img v-else src="../assets/images/icon-moon.svg" alt="Switch to dark mode"/>
    </div>
</template>

<script>
export default {
    name: 'ToggleBtn',
    data() {
        return {
            isDark: false,
        };
    },
    mounted() {
        // Initialise theme from saved preference or system
        const saved = localStorage.getItem('theme');
        const prefersDark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
        // if no saved preference, use system pref
        const initial = saved ? saved : (prefersDark ? 'dark' : 'light');
        this.applyTheme(initial);
    },
    methods: {
        applyTheme(theme) {
            // set dataset on root (document.documentElement) so index.scss :root[data-theme] overrides apply
            document.documentElement.dataset.theme = theme;
            this.isDark = theme === 'dark';
        },
        toggleMode() {
            const next = this.isDark ? 'light' : 'dark';
            this.applyTheme(next);
            try {
                localStorage.setItem('theme', next);
            } catch (e) {
                console.error('Could not save theme preference in local storage', e);
            }
        }
    }
}
</script>

<style scoped lang="scss">
.toggle-theme-btn {
    width: 30px;
    height: 30px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 35%;
    background-color: var(--tertiary);
    cursor: pointer;
}
div:hover {
    background-color: var(--quaternary);
}
</style>