<template>
	<!-- v-ifを使ってresultsがある時にだけ存在するという条件に応じて表示を変える-->
	<div class="results-country" v-if="results.country">{{ country }}</div>
	<div class="results-city" v-if="results.cityName">{{ cityName }}</div>
	<div class="results-temp" v-if="results.temperature">{{ temperature }}</div>
	<div class="results-condition" v-if="results.conditionText">
		<img :src="icon" alt="icon" />
		<span>{{ conditionText }}</span>
	</div>
</template>

<script setup>
//分割代入のため
import { toRefs } from "vue";

//結果（results）をApp.vueから受け取るイベント
//受け取りたいpropsであるresultsを書き加える
//Objectとは、受け取ったデータpropsの型（今回は複数の型を含んでいるのでObject型を指定）
const props = defineProps({
	results: Object,
});

//分割代入
const { country, cityName, temperature, icon, conditionText } = toRefs(
	props.results
);
</script>
