<template>
	<div>
		<h2>게시글 목록</h2>
		<hr class="my-4" />
		<PostFilter v-model:title="params.title_like" v-model="params._limit" />

		<hr class="my-4" />

		<AppGrid :items="posts" col-class="col-3">
			<template v-slot="{ item }">
				<PostItem
					:title="item.title"
					:content="item.content"
					:createdAt="item.createdAt"
					@click="goPage(item.id)"
				></PostItem>
			</template>
		</AppGrid>

		<AppPagination
			:current-page="params._page"
			:page-count="pageCount"
			@page="page => (params._page = page)"
		/>
		<template v-if="posts && posts.length > 0">
			<hr class="my-5" />
			<AppCard>
				<PostDetailView :id="posts[0].id"></PostDetailView>
			</AppCard>
		</template>
	</div>
</template>

<script setup>
import PostItem from '@/components/posts/PostItem.vue';
import AppCard from '@/components/AppCard.vue';
import AppPagination from '@/components/AppPagination.vue';
import PostFilter from '@/components/posts/PostFilter.vue';
import { getPosts } from '@/api/posts';
import { ref, watchEffect } from 'vue';
import { useRouter } from 'vue-router';
import { computed } from 'vue';
import PostDetailView from './PostDetailView.vue';
import AppGrid from '@/components/AppGrid.vue';

const router = useRouter();
const posts = ref([]);

const params = ref({
	_sort: 'createdAt',
	_order: 'desc',
	_page: 1,
	_limit: 6,
	title_like: '',
});
//pagination
const totalCount = ref(0);
const pageCount = computed(() =>
	Math.ceil(totalCount.value / params.value._limit),
);

const fetchPosts = async () => {
	try {
		//posts.value = getPosts();
		const { data, headers } = await getPosts(params.value);
		posts.value = data;
		totalCount.value = headers['x-total-count'];
	} catch (error) {
		console.error(error);
	}
};
//fetchPosts();
watchEffect(fetchPosts);

const goPage = id => {
	//router.push(`/posts/${id}`);
	router.push({
		name: 'PostDetail',
		params: { id },
	});
};
</script>
<style></style>
