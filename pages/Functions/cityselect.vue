<template>
	<view class="demo">
		<ss-select-city :hotCitys="hotCitys" v-model="value" @on-select="onSelect"></ss-select-city>
	</view>
</template>

<script>
	import ssSelectCity from '@/components/ss-select-city/ss-select-city.vue'
	export default {
		onShow: function() {
			this.value = getApp().globalData.cityname;
		},
		data() {
			return {
				hotCitys: ['杭州', '天津', '北京', '上海', '深圳', '广州', '成都', '重庆', '南京'],
				value: ""
			}
		},
		components: {
			ssSelectCity
		},
		methods: {
			onSelect(city) {
				console.log(city)
			}
		}
	}
</script>

<style lang="scss" scoped>

</style>
