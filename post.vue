
<template>
	<div class="contain">
		<div>
		{{$route.params.user_id}}
		<button @click="addPost">add post</button>
		</div>
		<div v-for="value, index in post" class="post">
			<h3><span>userId:</span> {{value.userId}}</h3>
			<h3><span>id:</span> {{value.id}} </h3>
			<h3><span>title</span> : {{value.title}}</h3>
			<p><span>body:</span> {{value.body}} </p>
			<button @click="del_post(index)">x</button>
			<button @click="gotoedit_post(index)">edit post</button>
		</div>
		<div id="add-post-form" v-if="add_more">
          <div class="form">
           	<!-- id
            <input v-model="tempPost.id"><br> -->
            title
            <input v-model="tempPost.title"><br>
            body
            <input v-model="tempPost.body"><br>
          </div>
        <button @click="submitNewPost()">Submit</button>
        <button @click="cancelSubmit()">Cancel</button>
      </div> 
	</div>
</template>
<script type="text/javascript">
export default{
	data: function(){
		return{
			tempPost: {
				userId: "",
    			id: "",
    			title: "",
    			body: ""},
			post:[],
			add_more: false,
		}
	},
	created: function(){
		var that =this;
		var post_data = window.localStorage.getItem("author_data");
		var post_array = JSON.parse(post_data);

		that.post = post_array[that.$route.params.user_id].post;
		var edit_post_data = JSON.stringify(that.post);
		window.localStorage.setItem("post",edit_post_data)
		console.log(post_array[that.$route.params.user_id].id);
	},
	methods: {
		back_post: function(){
			var that=this;
			that.$router.push({path:"/post/"})
		},
		gotoedit_post: function(index){
			var that=this;
      		that.$router.push({path: "/edit_post/"+ index});
    	},
    	del_post: function(index){
		var that = this;
		var post_data = window.localStorage.getItem("author_data");
		var post_array = JSON.parse(post_data);

		that.post.splice(index,1);
		post_array[that.$route.params.user_id].post = that.post

		var post_data = JSON.stringify(post_array);
		window.localStorage.setItem("author_data",post_data)
		var edit_post_data = JSON.stringify(that.post);
		window.localStorage.setItem("post",edit_post_data);
	    },
	   
	    cancelSubmit: function(){
	      var that = this;
	      that.add_more = false;
	      var post_data = window.localStorage.getItem("author_data");
			var post_array = JSON.parse(post_data);
  		},
	    submitNewPost: function(){
	      var that = this;
	      

	      var post_data = window.localStorage.getItem("author_data")
	     
	      if(post_data == null){
	        post_data = JSON.stringify([])
	      } 
	      console.log(that.post);
	      var post_array = JSON.parse(post_data)
	      that.tempPost.id = that.post.length + 1;
	      that.tempPost.userId = post_array[that.$route.params.user_id].id;
	      post_array[that.$route.params.user_id].post.push(that.tempPost)

	      

	      post_data = JSON.stringify(post_array)

	      window.localStorage.setItem("author_data",post_data)
	      var edit_post_data = JSON.stringify(that.post);
		  window.localStorage.setItem("post",edit_post_data) 

	      that.post = post_array[that.$route.params.user_id].post;
	      that.add_more = false;
	      
	    },
	    addPost: function(){
	    var that=this;
	    that.add_more=true;
// code dau rui?
   		 },

	}
}
</script>
<style>
.contain{
  display:inline-block;
}
.post {
  padding:5px;
  margin:5px;
  text-align:center; 
  display: inline-block;
  border:2px solid #4CAF50;
  border-radius:25px;
  float: left;
  width:380px;
  height:500px;
  /*position: relative;  */
}
/*.sm-box {
  width:300px ;
  height:300px;
  margin:auto;
}
.logout{
  padding: 0px 50px 0px 0px;
  display: inline-block;
  float: right;
}
button{
  margin-top: 10px;
  display: inline-block;
}
span{
  color: red;
}*/
h3{
  min-height: 26px;
  text-align: left;
}
p{
text-align: left;
}
</style>