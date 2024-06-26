<template>
	<div class="schedule">
		<template
			v-for="(roundMatches, roundNumber) in groupedMatches"
			:key="roundNumber">
			<div class="schedule__round">
				<h2>Kolejka {{ roundNumber }}</h2>
				<span
					v-if="roundNumber === currentRound.toString()"
					class="schedule__round__live"
					>ROZGRYWANA</span
				>
			</div>
			<div
				class="schedule__match"
				v-for="match in roundMatches"
				:key="match.id">
				<div class="schedule__match__player schedule__match__player__home">
					<span>{{ match.attributes.playerHome.data.attributes.name }} {{ match.attributes.playerHome.data.attributes.surname }}</span>
					<NuxtImg
						provider="strapi"
						class="players-card__image__avatar__image"
						:src="match.attributes.playerHome.data.attributes.club.data.attributes.logo.data.attributes.url"
						height="40" />
				</div>
				<div class="schedule__match__result">
					<template v-if="match.attributes.goalsHome === null && match.attributes.goalsAway === null"> VS </template>
					<template v-else-if="match.attributes.goalsHome === 0"> 0 : {{ match.attributes.goalsAway !== null ? match.attributes.goalsAway : 0 }} </template>
					<template v-else-if="match.attributes.goalsAway === 0"> {{ match.attributes.goalsHome !== null ? match.attributes.goalsHome : 0 }} : 0 </template>
					<template v-else> {{ match.attributes.goalsHome }} : {{ match.attributes.goalsAway }} </template>
				</div>
				<div class="schedule__match__player schedule__match__player__away">
					<NuxtImg
						provider="strapi"
						class="players-card__image__avatar__image"
						:src="match.attributes.playerAway.data.attributes.club.data.attributes.logo.data.attributes.url"
						height="40" />
					<span>{{ match.attributes.playerAway.data.attributes.name }} {{ match.attributes.playerAway.data.attributes.surname }}</span>
				</div>
			</div>
		</template>
	</div>
</template>

<script setup lang="ts">
const { find } = useStrapi();
const matchesResponse = await find("matches", {
	populate: "deep",
	pagination: {
		limit: 300,
	},
});
let matches: object[] = matchesResponse.data;

const groupedMatches = matches.reduce((result: object, element: object) => {
	const roundNumber: number = element.attributes.roundNumber;
	if (!result[roundNumber]) {
		result[roundNumber] = [];
	}
	result[roundNumber].push(element);
	return result;
}, {});

const currentRoundResponse = await find("current-round");
let currentRound: number = currentRoundResponse.data.attributes.round;
</script>

<style lang="scss" scoped>
.schedule {
	&__round {
		display: inline-flex;
		align-items: center;
		gap: rem(10);

		&__live {
			display: flex;
			background-color: #7fff00;
			color: $color-black;
			padding: rem(5) rem(15);
			border-radius: rem(25);
			text-align: center;
		}
	}
	&__match {
		display: grid;
		align-items: center;
		justify-content: center;
		gap: 1rem;
		grid-template-columns: 1fr 80px 1fr;
		background: $color-black-500;
		padding: rem(12) 0;

		&__player {
			display: flex;
			align-items: center;
			gap: rem(8);
			color: $color-black;

			&__home {
				justify-content: flex-end;
				text-align: right;
			}

			&__away {
				justify-content: flex-start;
				text-align: left;
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
}

@media (max-width: 430px) {
	.schedule {
		&__match {
			&__player {
				flex-direction: column-reverse;

				&__home {
					text-align: center;
				}

				&__away {
					text-align: center;
					flex-direction: column;
				}
			}
		}
	}
}
</style>
