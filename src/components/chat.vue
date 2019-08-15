<template>
	<div class="box">
		<div class="name">name:<input type="text" v-model="name"></div>
		<div ref="height" class="content-all">
			<li v-for="(item,index) in list"
				:key="index"
				:class="{red:item.name == name,bule:item.name!= name}"
			>
				{{item.name}}:{{item.msg}}
			</li>
		</div>
		<div class="content">内容:<input type="text" v-model="content" @keydown.enter="send"></div>
	</div>
</template>

<script>
	import axios from 'axios'
	export default {
		data() {
			return {
				name: '',
				content: '',
				list: []
			}
		},
		created() {
			this.getMsg()
		},
		methods: {
			send() {
				if (this.name != '' && this.content != '') {
					axios({
							method: 'GET',
							url: 'http://chat.duyiedu.com/sendMsg?name=' + this.name + '&msg=' + this.content
						})
						.then(res => {
							console.log(res)
							this.$refs.height.scrollTop = this.$refs.height.scrollHeight
						})
					this.content = '';
				}
			},
			getMsg() {
				//轮循 长连接 websock
				setInterval(()=>{
					axios({
						method: 'GET',
						url: 'http://chat.duyiedu.com/getMsgList'
					})
					.then(res=>{
						console.log(res.data)
						this.list = res.data
					})
				},1000)
				
			}
		}
	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	*{
		list-style: none;
	}
	.box {
		width: 500px;
		height: 800px;
		border: 1px solid red;
		margin: 0 auto;
	}
	.name {
		margin: 20px;
	}

	.content-all {
		width: 450px;
		height: 600px;
		margin: 20px auto;
		overflow-y: scroll;
	}
	.red{
		color: red;
	}
	.bule{
		color: bule;
	}
</style>
