<template>
	<div class="wrapper">
		<div class="container">
			<Title />
			<!-- @定義したイベント名で放出されたsubmitFormを受け取り、getWeatherを起動できるようにする -->
			<Form @submit-form="getWeather" />

			<!-- :をつけてイベントをpropsとして渡す -->
			<!--ここでの!は「Not」を意味し、「!false」であれば「Notfalse」、
      つまりtrue、「!true」であれば「Nottrue」、つまりfalseを意味します。 -->
			<Results :results="results" v-if="!loading" />
			<Loading v-if="loading" />
		</div>
	</div>
</template>

<script setup>
import { reactive, ref } from "vue";
import axios from "axios";
import "./assets/base.css";
import Title from "./components/Title.vue";
import Form from "./components/Form.vue";
import Results from "./components/Results.vue";
import Loading from "./components/Loading.vue";

//vueからrefを読み込み、それにloadingという名前を与え、初期状態の値としてfalseを保管しています。
//これはアプリの初期状態ではLoadingコンポーネントを表示させる必要はなく、
//weatherapiとの通信が始まってはじめて表示させるから
const loading = ref(false);

//データをResultsに送信する仕組み
//データの一時保管場所を作るのはref()もreactive()：version3から追加も同じ
const results = reactive({
	country: "",
	cityName: "",
	temperature: "",
	conditionText: "",
	icon: "",
});

//データを取得する仕組み
// .thenは送り返された気象データを受け取るコード
// resの中に気象データが入っている
const getWeather = (city) => {
	loading.value = true;
	axios
		.get(
			`https://api.weatherapi.com/v1/current.json?key=8e0aba82930040dc9e514612221209&q=${city}&aqi=no`
		)
		.then((res) => {
			(results.country = res.data.location.country),
				(results.cityName = res.data.location.name),
				(results.temperature = res.data.current.temp_c),
				(results.conditionText = res.data.current.condition.text),
				(results.icon = res.data.current.condition.icon);

			loading.value = false;
		})
		.catch((err) =>
			alert(
				"エラーが発生しました。ページをリロードして、もう一度トライしてください。"
			)
		);
};
</script>
