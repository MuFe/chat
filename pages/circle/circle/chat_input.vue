<template>
	<view class="footer">
		<!-- <view class="footer-left">
			<view class="uni-icon uni-icon-mic" @tap="startRecognize"> </view>
		</view> -->
		<view class="footer-center">
			
			<textarea
			auto-height="true"
			class="input-text"
			@confirm="sendMessge"
			v-model="inputValue"
		
			@blur="blur"
			:placeholder="placeholder"
			:maxlength="-1"
			:show-confirm-bar="false"
			style="max-height: 65upx;"
            cursor-spacing='15' 
            confirm-hold="true"
			/>
				
			<!-- <input class="input-text" type="text" @confirm="sendMessge" v-model="inputValue" :focus="focus" @blur="blur" :placeholder="placeholder" /> -->
			
		</view>
		<view class="footer-right">
			<view id='msg-type' class="send-comment" @tap="sendMessge">发送</view>
		</view>
	</view>
</template>

<script>
	export default {
		
		data() {
			return {
				inputValue: ''
			}
		},
		props:{
			placeholder: {
				type: String,
				required: true
			},
			focus: {
				type:Boolean,
				required: true
			}
		},
		onLoad(){
			
		},
		methods: {
			blur: function() {//失焦触发通知父组件
				var that = this;
				this.$emit('blur')
			},
			startRecognize: function () {
				var options = {};
				var that = this;
				options.engine = 'iFly';
				that.inputValue = "";
				plus.speech.startRecognize(options, function (s) {
					console.log(s);
					that.inputValue += s;
				}, function (e) {
					console.log("语音识别失败：" + e.message);
				});
			},
			sendMessge: function () {				
				if (!this.inputValue) {
					uni.showModal({
						content:"还没有输入内容哦!",
						showCancel:false
					})
					return;
				}
				//点击发送按钮时，通知父组件用户输入的内容
				this.$emit('send-message', this.inputValue);
				this.inputValue = '';//清空上次输入的内容
			}
		}
	}
</script>

<style>
	.footer {
		display: flex;
		flex-direction: row;
		width: 100%;
		height: 80upx;
		min-height: 80upx;
		border-top: solid 1px #bbb;
		overflow: hidden;
		padding: 5upx;
		background-color: #F4F5F6;
	}
	.footer-left {

		width: 80upx;
		height: 80upx;

		display: flex;
		justify-content: center;
		align-items: center;
	}
	.footer-right {
		width: 120upx;
		height: 80upx;
		display: flex;
		justify-content: center;
		align-items: center;
		color: #1482D1;
	}
	.footer-center {
		flex: 1;
		padding-left: 20upx;
		height: 80upx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.footer-center .input-text {
		flex: 1;
		background: #fff;
		/* border: solid 1upx #ddd; */
		padding: 10upx 14upx;
		font-family: verdana !important;
        font-size: 28upx;
        line-height:40upx;
		overflow: hidden;
		border-radius: 15upx;
        width: calc(100% - 28upx);
        
	}
	.footer-right .send-comment{
		background-color: #007AFF;
		text-align: center;
		line-height: 60upx;
		color: #FFFFFF;
		width: 80upx;
		height: 60upx;
		border-radius: 5px;
		font-size: 10px;
		/* height: 60upx; */
	}
</style>
