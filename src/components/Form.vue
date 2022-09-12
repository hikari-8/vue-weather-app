<template>
	<!-- ＠submitで　submitFormの仕組みを<form>と結びつける -->
	<!-- .preventでこれをつけないとデフォルトでページが再読み込みされてしまう -->
	<form @submit.prevent="submitForm">
		<!-- v-modelで、<input>タグに入力されたデータがcityへ書き込まれる -->
		<input type="text" v-model="city" placeholder="都市名を英語で入力" />

		<!-- <template>内では二重カッコ{{}}で挟むことで、
      constとして設定した値などをブラウザに表示させることができます -->
		<!-- {{ city }} -->

		<button type="submit">Get Weather</button>
	</form>
</template>

<script setup>
//refとはデータを一時保管する場所を作る特別な仕組み
import { ref } from "vue";

//データを一時保管する場所
const city = ref("");

//フォームを提出する仕組み
const submitForm = () => {
	//submitFormとsubmit-form(上位コンポーネントに対して放出したいイベント)と結びつける
	//ref()のデータに<script>タグ内からアクセスするには、.valueを使う必要がある
	//逆に、templateタグ内からアクセスする場合は、.valueは不要で、cityのみでOK
	emits("submit-form", city.value);
	//cityに保管されている都市名を初期化
	city.value = "";
};

//FormのsubmitFormからApp内のgetWeatherを起動させる仕組み(データを放出するイベントの定義)
//[ ]内の名前は、上位コンポーネントに対して放出したい仕組み（イベント）に近い名前
//（ここではsubmitFormとsubmitformなど）を使うのが一般的
const emits = defineEmits(["submit-form"]);
</script>
