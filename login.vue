<template>
	<div>
	Username:<input v-model="username">
	Password:<input type="password" v-model="password">
	<button @click="login()">login</button>
	</div>
</template>
<script>
export default {
	name:'login',
	data: function(){
		return{
			username: "",
			password: "", 
		} 
	},
	methods: { 
		login: function() {
			var that = this;
			if (that.username == 'tung' && that.password == '123456'){
				alert("login success");
				that.$router.push({path: '/user' });
			}else {
				alert("login failed");
			}
			that.username="";
			that.password="";
			//window.localStorage.removeItem(product_data)
		}
	}
}

</script>
<style>
</style>