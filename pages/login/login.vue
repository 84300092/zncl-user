<template>
	<view class="container">
		<image src="/static/login/loginbg.png" mode="" class="bg"></image>
		<view class="hello-area">
			<view class="hello">
				您好,
			</view>
			<view class="hello-text">
				欢迎登录车联网服务!
			</view>
		</view>
		<view class="input-area">
			<uni-forms :modelValue="formData">
				<uni-forms-item class="phone-input">
					<view class="input-border">
						<uni-easyinput type="text" v-model="formData.username" placeholder="请输入手机号"
							:inputBorder="false" />
					</view>
				</uni-forms-item>
				<uni-forms-item class="yzm-input">
					<view class="input-border">
						<uni-easyinput type="password" v-model="formData.password" placeholder="请输入验证码"
							:inputBorder="false">
							<template #right>
								<button class="getyzm">获取验证码</button>
							</template>
						</uni-easyinput>
					</view>

				</uni-forms-item>
			</uni-forms>
			<button @click="submitForm" class="login">登录</button>
		</view>
		<image src="/static/login/loginp1.png" mode="" class="loginp1"></image>
	</view>
</template>

<script setup>
	import {
		ref
	} from 'vue'
	const formData = ref({
		username: '',
		password: ''
	})
</script>

<style scoped>
	.container {
		max-width: 750rpx;
		background-color: #eeeeee;
		position: relative;
		z-index: 1;
	}

	.bg {
		width: 100%;
		height: 200px;
		position: absolute;
		z-index: -1;
		top: 0;
		right: 0;
	}

	.loginp1 {
		position: absolute;
		z-index: 2;
		right: 30rpx;
		top: 160rpx;
		width: 250rpx;
		height: 220rpx;
	}

	.hello-area {
		padding-left: 100rpx;
		padding-top: 130rpx;
		color: #fff;
	}

	.hello {
		font-size: 40rpx;
		margin-bottom: 10rpx;
	}

	.hello-text {
		font-size: 30rpx;
	}

	.input-area {
		background-color: #fff;
		height: 100vh;
		margin-top: 80rpx;
		border-radius: 50rpx;
		padding: 120rpx 100rpx;
		
	}
	.input-border{
		border-bottom: 1rpx solid #eeeeee;
	}
	.getyzm {
		width: 180rpx;
		height: 60rpx;
		background-color: #007AFF;
		color: #fff;
		border-radius: 30rpx;
		font-size: 24rpx;
		margin-bottom: 10rpx;
	}

	.login {
		max-width: 500rpx;
		height: 90rpx;
		background-color: #007AFF;
		color: #fff;
		border-radius: 40rpx;
		margin-top: 80rpx;
	}
</style>