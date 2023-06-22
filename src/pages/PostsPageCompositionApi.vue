<template>
    <div>
        <h1>Posts page</h1>

        <app-input 
            v-focus
            v-model="searchQuery"
            placeholder="Search..."
        />
        
        <app-button
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
            />
        </app-dialog>
    
        <post-list 
            :posts="sortedAndSearchedPosts"
            v-if="!isPostsLoading" 
        />
        <div v-else>Loading...</div>
    </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import AppButton from '@/components/UI/AppButton.vue';
import AppSelect from '@/components/UI/AppSelect.vue';
import axios from 'axios';
import AppInput from '@/components/UI/AppInput.vue';
import {ref} from 'vue';
import {usePosts} from "@/hooks/usePosts";
import useSortedPosts from "@/hooks/useSortedPosts";
import useSortedAndSearchedPosts from "@/hooks/useSortedAndSearchedPosts";

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
            sortOptions: [
                {value: 'title', name: 'By name'},
                {value: 'body', name: 'By content'},
            ],
        }
    },

    setup(props) {
    const {posts, totalPages, isPostsLoading} = usePosts(10);
    const {sortedPosts, selectedSort} = useSortedPosts(posts);
    const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts)
    return {
      posts,
      totalPages,
      isPostsLoading,
      sortedPosts,
      selectedSort,
      searchQuery,
      sortedAndSearchedPosts,
    }
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