<template>
	<view>
		<uni-forms ref="form" :modelValue="formData">
			<uni-forms-item label="手机">
				<uni-easyinput v-model="formData.phone" placeholder="手机"></uni-easyinput>
			</uni-forms-item>
			<uni-forms-item label="密码">
				<uni-easyinput type="password" v-model="formData.password" placeholder="请输入密码"></uni-easyinput>
			</uni-forms-item>
			<uni-forms-item label="数值">
				<uni-easyinput type="number"  v-model="formData.steps" placeholder="请输入数值"></uni-easyinput>
			</uni-forms-item>
		</uni-forms>
		<button @click="submit">确定</button>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				formData: {
					phone: '',
					password: '',
					steps: 0
				},
			}

		},
		onLoad() {
			var that = this
			uni.getStorage({
			    key: 'phone',
			    success: function (res) {
			        that.formData.phone = res.data
			    }
			});
			uni.getStorage({
			    key: 'password',
			    success: function (res) {
			        that.formData.password = res.data
			    }
			});
			uni.getStorage({
			    key: 'steps',
			    success: function (res) {
					that.formData.steps = res.data
			    }
			});
			
		},
		methods: {
			submit() {
				this.$refs.form.validate().then(res => {
					//console.log('表单数据信息：', res);
					var data = {
						user: res.phone,
						password: res.password,
						steps: res.steps
					}
					uni.setStorage({
					    key: 'phone',
					    data: res.phone,
					});
					uni.setStorage({
					    key: 'password',
					    data: res.password,
					});
					uni.setStorage({
					    key: 'steps',
					    data: res.steps,
					});
					uniCloud.callFunction({
							name: 'xiaomi',
							data: data
						})
						.then(res => {
							uni.showModal({
							    title: '提示',
								showCancel:false,
							    content: res.result || "空白",
							});
							
						});

				}).catch(err => {
					console.log('表单错误信息：', err);
				})
			}


		}
	}
</script>
