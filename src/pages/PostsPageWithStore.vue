<template>
    <div>
        <h1>Posts page</h1>

        <app-input 
            v-focus
            :model-value="searchQuery"
            @update:model-value="setSearchQuery"
            placeholder="Search..."
        />
        
        <app-button
            @click="showDialog"
        >
            Create Post
        </app-button>

        <app-select 
            :model-value="selectedSort"
            @update:model-value="setSelectedSort"
            :options="sortOptions"
        />

        <app-dialog
            v-model:show="dialogVisible"
        >
            <post-form 
                @create="createPost" 
            />
        </app-dialog>
    
        <post-list 
            :posts="sortedAndSearchedPosts"
            @remove="removePost"
            v-if="!isPostsLoading" 
        />
        <div v-else>Loading...</div>

        <div v-intersection="loadMorePosts" class="observer"></div>
    </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import AppButton from '@/components/UI/AppButton.vue';
import AppSelect from '@/components/UI/AppSelect.vue';
import AppInput from '@/components/UI/AppInput.vue';
import {mapActions, mapGetters, mapState, mapMutations} from 'vuex';
import axios from 'axios';

export default {
    components: {
        PostForm,
        PostList,
        AppButton,
        AppSelect,
        AppInput,
    },
    data() {
        return{
            dialogVisible: false,
        }
    },

    methods: {
        ...mapMutations({
            setPage: 'post/setPage',
            setSearchQuery: 'post/setSearchQuery',
            setSelectedSort: 'post/setSelectedSort',
        }),
        ...mapActions({
            loadMorePosts: 'post/loadMorePosts',
            fetchPosts: 'post/fetchPosts',

        }),
        createPost(post) {
            this.posts.push(post);
            this.dialogVisible = false;
        },
        removePost(post) {
            this.posts = this.posts.filter(p => p.id !== post.id)
        },
        showDialog() {
            this.dialogVisible = true;
        },
    },

    mounted() {
        this.fetchPosts();
    },

    computed: {
        ...mapState({
            posts: state => state.post.posts,
            isPostsLoading: state => state.post.isPostsLoading,
            searchQuery: state => state.post.searchQuery,
            selectedSort: state => state.post.selectedSort,
            totalPages: state => state.post.totalPages,
            page: state => state.post.page,
            limit: state => state.post.limit,
            sortOptions: state => state.post.sortOptions,
        }),
        ...mapGetters({
            sortedPosts: 'post/sortedPosts',
            sortedAndSearchedPosts: 'post/sortedAndSearchedPosts',
        }),
    },

    watch: {
        // page() {
        //     this.fetchPosts()
        // }
    },
}
</script>

<style>
.page--wrapper {
    display: flex;
    justify-content: center;
    margin: 20px 0 0 0;
}

.page {
    width: 30px;
    height: 30px;
    border: 1px solid #000;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
}
.page:not(:last-of-type) {
    margin: 0 10px 0 0;
}

.current_page {
    border: 1px solid #0ed429;
}
</style>