<template>
	<div class="inform_pr">
		<div>
			<h3>author {{edit_author.id}}: {{edit_author.name}} </h3>
			<p>username: {{edit_author.username}} </p>
			<p>email: {{edit_author.email}}</p>
			<p>address:
			street: {{edit_author.address.street}} 
		    suite: {{edit_author.address.suite}}
			city: {{edit_author.address.city}}
			zipcode: {{edit_author.address.zipcode}}</p>
			<p> geo:
			 lat: {{edit_author.address.geo.lat}}
			 lng: {{edit_author.address.geo.lng}} </p>
			<h3> phone: {{edit_author.phone}} </h3>
			<h3> website: {{edit_author.website}} </h3>
			<h3> company: {{edit_author.company.name}} </h3>
			<p> catchPhrase: {{edit_author.company.catchPhrase}} </p>
			<p> bs: {{edit_author.company.bs}} </p>
			<button @click="editAuthor()">edit</button>
		</div>
		<div v-if="edited">
			<p>username:<input v-model="temp_edit_author.username"></p>
			<p>email:<input v-model="temp_edit_author.email"></p>
			<p>phone:<input v-model="temp_edit_author.phone"></p>
			<button @click="change_author()">Change</button>
			<button @click="cancel_edit()">Cancel Edit</button>
			<button @click="goToAddPost()">edit post</button>
		</div>
		<div class="go_back">
			<button @click="back_user">Back</button>
		</div>
	</div>
</template>
<script type="text/javascript">
export default{
	data: function(){
		return{
			edit_author: {},
			edited: false,
			temp_edit_author: {}
		}
	},
	created: function(){
		var that=this;
		console.log(that.$route)
		console.log(that.$router);
		var edit_author_data = window.localStorage.getItem("author_data")
		var edit_author_array = JSON.parse(edit_author_data)
		that.edit_author = edit_author_array[that.$route.params.author_id]
	},
	methods: {
		back_user: function(){
			var that=this;
			that.$router.push({path:"/user/"})
		},
		editAuthor: function(){
			var that= this;
			that.edited = true;
			var edit_author_data = window.localStorage.getItem("author_data")
			var edit_author_array = JSON.parse(edit_author_data)
			that.temp_edit_author = edit_author_array[that.$route.params.author_id]
		},
		cancel_edit: function(){
			var that= this;
			that.edited = false;
		},
		change_author: function(){
			var that= this;
			var edit_author_data = window.localStorage.getItem("author_data")
			var edit_author_array = JSON.parse(edit_author_data)
		    edit_author_array[that.$route.params.author_id] = that.temp_edit_author
		    edit_author_data = JSON.stringify(edit_author_array)
		    window.localStorage.setItem("author_data",edit_author_data)
		    that.edit_author = edit_author_array[that.$route.params.author_id]
		    that.edited = false;
		},
		goToAddPost: function(){
			var that=this;
			that.$router.push('/post/' + that.$route.params.author_id)
		}


	}
}
</script>
<style> 
.sm-box{
	width:300px ;
  	height:300px;
 }
.inform_pr {
	display:inline-block;
	text-align:center;
}
</style> 