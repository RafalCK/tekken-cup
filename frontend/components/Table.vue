<template>
	<div class="table">
		<table class="table__table">
			<tr>
				<th width="1%">#</th>
				<th>Zawodnik</th>
				<th>{{ isShort ? "M" : "Mecze" }}</th>
				<th v-if="!isShort">{{ isShort ? "Z" : "Zwycięstwa" }}</th>
				<th v-if="!isShort">{{ isShort ? "WD" : "Wygrana dogrywka" }}</th>
				<th v-if="!isShort">{{ isShort ? "P" : "Przegrane" }}</th>
				<th v-if="!isShort">{{ isShort ? "PD" : "Przegrana dogrywka" }}</th>
				<th>{{ isShort ? "B" : "Bilans" }}</th>
				<th>{{ isShort ? "P" : "Punkty" }}</th>
			</tr>
			<tbody>
				<tr
					v-for="(summary, index) in summaries"
					:key="summary.id">
					<td>{{ index + 1 }}</td>
					<td class="table__table__player">
						<NuxtImg
							provider="strapi"
							class="header__logo__logo"
							:src="summary.attributes.player.data.attributes.club.data.attributes.logo.data.attributes.url"
							height="20" />
						{{ summary.attributes.player.data.attributes.name }} {{ summary.attributes.player.data.attributes.surname }}
					</td>
					<td>{{ summary.attributes.matches }}</td>
					<td v-if="!isShort">{{ summary.attributes.wins }}</td>
					<td v-if="!isShort">{{ summary.attributes.drawsWin }}</td>
					<td v-if="!isShort">{{ summary.attributes.lost }}</td>
					<td v-if="!isShort">{{ summary.attributes.drawsLost }}</td>
					<td>{{ summary.attributes.goalsScored }}-{{ summary.attributes.goalsLost }}</td>
					<td class="table__table__points">{{ summary.attributes.wins * 3 + summary.attributes.drawsWin * 2 + summary.attributes.drawsLost * 1 }}</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script setup lang="ts">
const props = defineProps({
	kind: {
		type: String,
		required: false,
		default: "long",
	},
});

const { find } = useStrapi();
const summariesResponse = await find("summaries", {
	populate: "deep",
});

let summaries = summariesResponse.data.sort((a, b) => {
	const scoreA = a.attributes.wins * 3 + a.attributes.drawsWin * 2 + a.attributes.drawsLost * 1;
	const scoreB = b.attributes.wins * 3 + b.attributes.drawsWin * 2 + b.attributes.drawsLost * 1;

	const goalDifferenceA = a.attributes.goalsScored - a.attributes.goalsLost;
	const goalDifferenceB = b.attributes.goalsScored - b.attributes.goalsLost;

	if (scoreA === scoreB) {
		return goalDifferenceB - goalDifferenceA;
	}
	return scoreB - scoreA;
});

const isShort = computed(() => props.kind === "short" || false);
</script>

<style lang="scss" scoped>
.table {
	display: flex;
	justify-content: flex-start;
	background: $color-black-500;
	border-bottom: rem(6) solid $color-primary;
	color: $color-black;
	overflow-x: auto;

	&__table {
		width: 100%;
		tr,
		th,
		td {
			padding: rem(10);
		}

		th {
			text-align: left;
		}

		&__points {
			color: $color-primary;
		}

		&__player {
			display: inline-flex;
			align-items: center;
			gap: rem(5);
		}
	}
}

@media (max-width: 350px) {
	.table {
		&__table {
			tr,
			th,
			td {
				padding: rem(10) rem(5);
			}
		}
	}
}
</style>
