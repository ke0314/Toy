<template>
	<div>
		<h2>게시글 목록</h2>
		<hr class="my-4" />
		<div class="row g-3">
			<dib v-for="post in posts" :key="post.id" class="col-4">
				<PostItem
					:title="post.title"
					:content="post.content"
					:createdAt="post.createdAt"
					@click="goPage(post.id)"
				></PostItem>
			</dib>
			<hr class="my-4" />
			<AppCard>
				<PostDetailView :id="1"></PostDetailView>
			</AppCard>
		</div>
	</div>
</template>

<script setup>
import PostItem from '@/components/posts/PostItem.vue';
import AppCard from '@/components/AppCard.vue';
import { getPosts } from '@/api/posts';
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import PostDetailView from './PostDetailView.vue';
const router = useRouter();
const posts = ref([]);
const fetchPosts = () => {
	posts.value = getPosts();
};
fetchPosts();
const goPage = id => {
	//router.push(`/posts/${id}`);
	router.push({
		name: 'PostDetail',
		params: { id },
	});
};
</script>
<style></style>
