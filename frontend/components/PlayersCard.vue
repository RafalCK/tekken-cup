<template>
	<div class="players-card">
		<div class="players-card__player">
			<div class="players-card__player__image">
				<NuxtImg
					provider="strapi"
					class="players-card__player__image__image"
					:src="item.avatar.data.attributes.url"
					height="50" />
			</div>
			<div class="players-card__player__name">
				<span class="players-card__player__name__name">{{ item.name }}</span>
				<span class="players-card__player__name__surname">{{ item.surname }}</span>
			</div>
		</div>
		<div class="players-card__character">
			<NuxtImg
				provider="strapi"
				class="players-card__character__image"
				:src="item.club.data.attributes.logo.data.attributes.url" />
			<span class="players-card__character__name">{{ item.club.data.attributes.name }}</span>
		</div>
	</div>
</template>

<script setup lang="ts">
const props = defineProps({
	item: {
		type: Object,
		required: true,
	},
});
</script>

<style scoped lang="scss">
.players-card {
	display: flex;
	align-items: center;
	justify-content: space-between;
	background: $color-black-500;
	border-bottom: rem(6) solid $color-primary;
	box-sizing: border-box;

	&__player {
		display: flex;
		flex-direction: row;
		padding: rem(30) rem(10) rem(30) rem(20);
		&__image {
			margin-right: rem(10);
			&__image {
				margin-right: rem(10);
				border-radius: 50%;
			}
		}

		&__name {
			display: flex;
			flex-direction: row;
			font-size: rem(20);
			gap: rem(5);
			align-items: center;

			span {
				display: block;
			}
		}
	}

	&__character {
		display: flex;
		height: 100%;
		max-width: rem(220);
		position: relative;
		&__image {
			width: 100%;
			&__image {
			}
		}

		&__name {
			font-size: rem(14);
			position: absolute;
			right: rem(5);
			bottom: rem(5);
			text-shadow: 0 0 rem(5) $color-black;
		}
	}
}

@media (max-width: 600px) {
	.players-card {
		&__name {
			font-size: rem(20);
		}
	}
}
</style>
