<template>
    <div>
        <h1>Posts page</h1>

        <app-input 
            v-focus
            v-model="searchQuery"
            placeholder="Search..."
        />
        
        <app-button
            @click="showDialog"
        >
            Create Post
        </app-button>

        <app-select 
            v-model="selectedSort"
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

        <!-- <div class="page--wrapper">
            <div
                v-for="pageNumber in totalPages"
                :key="pageNumber"
                class="page"
                :class="{
                    'current_page': page === pageNumber
                }"
                @click="changePage(pageNumber)"
            >
                <span class="page_number">
                    {{ pageNumber }}
                </span>
            </div>
        </div> -->
    </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import AppButton from '@/components/UI/AppButton.vue';
import AppSelect from '@/components/UI/AppSelect.vue';
import axios from 'axios';
import AppInput from '@/components/UI/AppInput.vue';

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
            posts: [],
            dialogVisible: false,
            isPostsLoading: false,
            searchQuery: '',
            selectedSort: '',
            totalPages: 0,
            page: 1,
            limit: 10,
            sortOptions: [
                {value: 'title', name: 'By name'},
                {value: 'body', name: 'By content'},
            ]
        }
    },

    methods: {
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
        // changePage(pageNumber) {
        //     this.page = pageNumber;
        // },
        async fetchPosts() {
            try {
                this.isPostsLoading = true;
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params: {
                        _page: this.page,
                        _limit: this.limit,
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = response.data;
            } catch(e) {
                alert('Error!!!')
            } finally {
                this.isPostsLoading = false;
            }
        },
        async loadMorePosts() {
            try {
                this.page += 1;
                const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
                    params: {
                        _page: this.page,
                        _limit: this.limit,
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
                this.posts = [...this.posts, ...response.data];
            } catch(e) {
                alert('Error!!!')
            }
        },
    },

    mounted() {
        this.fetchPosts();
        // const options = {
        //     rootMargin: '0px',
        //     threshold: 1.0
        // }
        // const callback = (entries, observer) => {
        //     if (entries[0].isIntersecting && this.page < this.totalPages) {
        //         this.loadMorePosts();
        //     }
        // };
        // const observer = new IntersectionObserver(callback, options);
        // observer.observe(this.$refs.observer);
    },

    computed: {
        sortedPosts() {
           return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
            ) 
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
        }
    },

    watch: {
        // page() {
        //     this.fetchPosts()
        // }
    },
}
</script>

<style lang="scss" scoped>
.page {
    width: 30px;
    height: 30px;
    border: 1px solid #000;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;

    &:not(:last-of-type) {
        margin: 0 10px 0 0;
    }

    &--wrapper {
        display: flex;
        justify-content: center;
        margin: 20px 0 0 0;
    }
}

.current_page {
    border: 1px solid #0ed429;
}
</style>