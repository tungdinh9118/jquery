<template>
	<div class="contain">
		<div>
			<button @click="load_data()">Load Data</button>
			<button @click="add_author()">add author</button>
		    <button @click="logout()">logout</button>
		</div>
		<div v-for="author,index in author" class="author">
			<h3>author {{author.id}}: {{author.name}} </h3>
			<p>username: {{author.username}} </p>
			<p>email: {{author.email}}</p>
			<p>address:
			street: {{author.address.street}} 
		    suite: {{author.address.suite}}
			city: {{author.address.city}}
			zipcode: {{author.address.zipcode}}</p>
			<p> geo:
			 lat: {{author.address.geo.lat}}
			 lng: {{author.address.geo.lng}} </p>
			<h3> phone: {{author.phone}} </h3>
			<h3> website: {{author.website}} </h3>
			<h3> company: {{author.company.name}} </h3>
			<p> catchPhrase: {{author.company.catchPhrase}} </p>
			<p> bs: {{author.company.bs}} </p>
			<button @click="move_post(index)" >post of author</button>
			<button @click="edit_author(index)">edit author</button>
			<button @click="delauthor(index)">x</button>
		</div>
		 <div id="add-author-form" v-if="add_more">
          <div class="form">
            Name
            <input v-model="xxx.name"><br>
            Email
            <input v-model="xxx.email"><br>
            Phone
            <input v-model="xxx.phone"><br>
            username
            <input v-model="xxx.username"><br>
            <!-- id:
            <input v-model="xxx.id"> <br> -->
          </div>
        <button @click="submitNewAuthor()">Submit</button>
        <button @click="cancelSubmit()">Cancel</button>
      </div>

	</div>
</template>
<script type="text/javascript">
export default{
	data: function(){
		return{
			tempAuthor: [
				{
				 post:[],  	
				},
			],
			tempPost:[],
			author: [],
			xxx:{
				id: "",
			    name: "",
			    username: "",
			    email: "",
			    address: {
				    street: "",
				    suite: "",
				    city: "",
				    zipcode: "",
					    geo: {
					        lat: "",
					        lng: "",
						},
		 			},
		 			phone: "",
				    website: "",
				    company: {
				      name: "",
				      catchPhrase: "",
				      bs: ""
				    },
				    post: []
		 	},
			add_more: false,
		
	}
	},
	created: function(){
		var that = this;
		$.ajax({
			url: 'https://jsonplaceholder.typicode.com/users/',
			method: 'GET',
			data: {

			},
			success: function(data){ 
				that.$set(that,"tempAuthor",data);
				console.log("sucess");
			},
			error: function(err){
				console.log("error");
			}
		});
		$.ajax({
			url: 'https://jsonplaceholder.typicode.com/posts/',
			method: 'GET',
			data: {

			},
			success: function(data){ 
				that.$set(that,"tempPost",data);

			},
			error: function(err){
				console.log("error");
			}
		});
		  var author_data = window.localStorage.getItem("author_data");
		  if (author_data == null){
		    author_data = JSON.stringify([]);
		  }
		  var author_array = JSON.parse(author_data);
		  that.author = author_array;
	},
	methods: {
		move_post: function(index){
			var that = this;
			that.$router.push({path: "/post/" + index});
		},
		load_data: function(){
			var that = this;
			var i;
			var j;
			console.log(that.author);
		 	console.log(that.tempAuthor);
		  	console.log(that.tempAuthor[0].id);
		 	console.log(that.tempPost[0].userId);

		for (i = 0; i < that.tempAuthor.length; ++i ){
			 var x = [];
			for ( j = 0; j < that.tempPost.length; ++j ){
				if( that.tempAuthor[i].id == that.tempPost[j].userId ){
					 x.push(that.tempPost[j]);
				}
			}
			that.tempAuthor[i].post = x;
		}
		var user_json = JSON.stringify(that.tempAuthor);
		window.localStorage.setItem("author_data",user_json);
		var author_data = window.localStorage.getItem("author_data");
		  if (author_data == null){
		    author_data = JSON.stringify([]);
		  }
		  var author_array = JSON.parse(author_data);
		  that.author = author_array;

		console.log(that.author);
		},
	
	// edit_author: function(id){
	// 	var that= this;
	// 	that.$router.push({path: '/edit/' + id })
	// },
	delauthor: function(index){
		var that = this;
		that.author.splice(index,1);
		var author_data = JSON.stringify(that.author);
		window.localStorage.setItem("author_data",author_data);
	},
	edit_author: function(index){
		var that = this;
		that.$router.push({path: "/edit_author/"+ index});
	},
    cancelSubmit: function(){
      var that = this;
      that.add_more = false;
      that.xxx = {
				id: "",
			    name: "",
			    username: "",
			    email: "",
			    address: {
				    street: "",
				    suite: "",
				    city: "",
				    zipcode: "",
					    geo: {
					        lat: "",
					        lng: "",
						},
		 			},
		 			phone: "",
				    website: "",
				    company: {
				      name: "",
				      catchPhrase: "",
				      bs: ""
				    },
				    post: []
		 	};
    },
    submitNewAuthor: function(){
      var that = this;
      

      var author_data = window.localStorage.getItem("author_data")
     
      if(author_data == null){
        author_data = JSON.stringify([])
      } 
      var author_array = JSON.parse(author_data)
      that.xxx.id = that.author.length 

      author_array.push(that.xxx)

      author_data = JSON.stringify(author_array)

      window.localStorage.setItem("author_data",author_data) 

      that.author = author_array;
      that.add_more = false;
      that.xxx = {
				id: "",
			    name: "",
			    username: "",
			    email: "",
			    address: {
				    street: "",
				    suite: "",
				    city: "",
				    zipcode: "",
					    geo: {
					        lat: "",
					        lng: "",
						},
		 			},
		 			phone: "",
				    website: "",
				    company: {
				      name: "",
				      catchPhrase: "",
				      bs: ""
				    },
				    post: []
		 	};
    },
    logout: function(){
      var that = this;
      that.$router.push('/login')

    },
    add_author: function(){
    var that=this;
    that.add_more=true;

    },
	}
}
</script>
<style>
.contain{
  display:block;
}
.addsp{
  text-align: center;
  display:block;
  margin: 30px 0px 20px 0px;

}
.author {
  padding:5px;
  margin:auto;
  text-align:center; 
  display: inline-block;
  border:2px solid #4CAF50;
  border-radius:25px;
  width:380px;
  height:600px;
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