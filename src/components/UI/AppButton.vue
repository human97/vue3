<template>
    <button class="btn">
        <slot/>
    </button>
</template>

<script>
export default {
    name: 'app-button'
}
</script>

<style scoped>
.btn {
    height: 40px;
    padding: 0 20px;
    background: transparent;
    border: 2px solid #0bbb0e;
    border-radius: 10px;
    font-size: 16px;
    font-weight: 700;
    color: #0bbb0e;
    cursor: pointer;
}
</style>