<template>
    <navbar/>
    <div class="app">
        <router-view></router-view>
    </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue'
export default {
  components: { Navbar },
    
}
</script>

<style lang="scss">
*,
*:before,
*:after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    outline: none;
}

.app {
    padding: 0 10px 10px 10px;
}
</style>
