<template>
    <div class="navbar">
        <div class="navbar--btn" @click="$router.push('/')">My App</div>
        <div>
            <app-button class="m-r" @click="$router.push('/posts')">Posts</app-button>
            <app-button class="m-r" @click="$router.push('/about')">About</app-button>
            <app-button class="m-r" @click="$router.push('/store')">Store</app-button>
            <app-button class="m-r" @click="$router.push('/composition')">Composition API</app-button>
        </div>
    </div>
</template>

<script>
import AppButton from '@/components/UI/AppButton.vue';
export default {
  components: { AppButton },
    
}
</script>

<style scoped>
.navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 50px;
    background: #d0d8d3;
    padding: 0 10px;
}

.navbar--btn {
    cursor: pointer;
}

.m-r:not(:last-of-type) {
    margin: 0 10px 0 0;
}
</style>