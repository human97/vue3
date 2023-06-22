<template>
    <form @submit.prevent>
            <h3>Create post</h3>

            <app-input
                v-focus 
                v-model="post.title"
                placeholder="Name"
            />
            <textarea 
                v-model="post.body" 
                name="" 
                id="" 
                cols="30" 
                rows="10" 
                placeholder="Discription"
            />

            <app-button
                @click="createPost"
            >
                Create
            </app-button>
        </form>
</template>

<script>
export default {
    data() {
        return {
            post: {
              title: '',
              body: '',  
            }
        }
    },

    methods: {
        createPost() {
            this.post.id = Date.now();
            this.$emit('create', this.post);
            this.post = {
                title: '',
                body: '',
            }
        }
    },
}
</script>

<style scoped>

</style>