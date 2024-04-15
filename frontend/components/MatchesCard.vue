<template>
	<div class="matches-card">
		<div class="matches-card__player matches-card__player__home">
			<NuxtImg
				class="matches-card__player__image"
				provider="strapi"
				:src="item.attributes.playerHome.data.attributes.club.data.attributes.logo.data.attributes.url"
				height="80" />
			<span class="matches-card__player__name matches-card__player__name__home">{{ item.attributes.playerHome.data.attributes.club.data.attributes.name }}</span>
			<span>{{ item.attributes.playerHome.data.attributes.name }} {{ item.attributes.playerHome.data.attributes.surname }}</span>
		</div>
		<div class="matches-card__result">{{ item.attributes.goalsHome !== null ? (item.attributes.goalsHome !== 0 ? item.attributes.goalsHome : "0") : "-" }} : {{ item.attributes.goalsAway !== null ? (item.attributes.goalsAway !== 0 ? item.attributes.goalsAway : "0") : "-" }}</div>
		<div class="matches-card__player matches-card__player__away">
			<span>{{ item.attributes.playerAway.data.attributes.name }} {{ item.attributes.playerAway.data.attributes.surname }}</span>
			<NuxtImg
				class="matches-card__player__image"
				provider="strapi"
				:src="item.attributes.playerAway.data.attributes.club.data.attributes.logo.data.attributes.url"
				height="80" />
			<span class="matches-card__player__name matches-card__player__name__away">{{ item.attributes.playerAway.data.attributes.club.data.attributes.name }}</span>
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

<style lang="scss" scoped>
.matches-card {
	display: grid;
	position: relative;
	align-items: center;
	justify-content: center;
	gap: 1rem;
	grid-template-columns: 1fr 80px 1fr;
	background: $color-black-500;
	border-bottom: rem(6) solid $color-primary;
	box-sizing: border-box;
	z-index: -1;

	&__player {
		display: flex;
		align-items: center;
		gap: rem(8);
		color: $color-black;

		span {
			text-align: center;
		}

		&__home {
			justify-content: space-between;
		}

		&__away {
			justify-content: space-between;
		}

		&__image {
			position: relative;
		}

		&__name {
			font-size: rem(14);
			color: $color-white;
			position: absolute;
			bottom: rem(5);
			text-shadow: 0 0 rem(5) $color-black;

			&__home {
				left: rem(5);
			}

			&__away {
				right: rem(5);
			}
		}
	}

	&__result {
		text-align: center;
		background: $color-primary;
		padding: rem(4) 0 rem(4) 0;
		margin: 0 rem(8) 0 rem(8);
		color: $color-black-500;
	}
}
</style>
