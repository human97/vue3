<template>
    <div 
        class="dialog" 
        v-if="show"
        @click="hideDialog" 
    >
        <div 
            class="dialog--content"
            @click.stop
        >
            <slot></slot>
        </div>
    </div>
</template>

<script>
import toggleMixins from "@/mixins/toggleMixin";

export default {
   name: 'app-dialog',
   mixins: [toggleMixins],
}
</script>

<style scoped>
.dialog {
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    background: rgba(0,0,0, .5);
    position: fixed;
    display: flex;
}

.dialog--content {
    margin: auto;
    background: #fff;
    border-radius: 10px;
    min-height: 50px;
    min-width: 300px;
    padding: 20px;
}
</style>