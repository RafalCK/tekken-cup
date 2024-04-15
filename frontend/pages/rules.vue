<template>
	<div class="rules">
		<div v-html="rulesResponse.data.attributes.content"></div>
	</div>
</template>

<script setup lang="ts">
const { find } = useStrapi();
const rulesResponse = await find("rule");
</script>

<style lang="scss" scoped>
.rules {
	background: $color-black-500;
	color: $color-black;
	padding: rem(20);
}
:deep(li) {
	line-height: 28px;
}
</style>
