<template>
    <div class="post">
        <div>
            <span>ID: {{ post.id }}.</span>
            <h3>Name: {{ post.title }}</h3>
            <p class="post_discription">Discription: {{ post.body }}</p>
        </div>

        <div class="post--btn">
            <app-button
                class="m-r"
                @click="$router.push(`/posts/${post.id}`)"
            >
                Open
            </app-button>
            <app-button
                class="m-r"
                @click="$emit('remove', post)"
            >
                Delete
            </app-button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        post: {
            type: Object,
            required: true,
        }
    }
}
</script>

<style lang="scss" scoped>
.post {
    margin: 10px 0 0 0;
    padding: 10px;
    border: 3px solid #0bbb0e;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;

    &--btn {
        display: flex;
    }
}

.m-r:not(:last-of-type) {
 margin: 0 10px 0 0;
}
</style>