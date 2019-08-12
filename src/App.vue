<template>
    <div id="app">
        <headerList></headerList>
        <sideBarList></sideBarList>
    </div>
</template>

<script>
    import headerList from './components/headerList.vue'
    import sideBarList from './components/sideBarList.vue'

    export default {
        name: 'app',
        components: {headerList, sideBarList}
    }
</script>

<style>
</style>
