<template>
  <div class="contain">
    {{$route.params.post_id}}
    <div>
      <p>userId {{edit_post.userId}}</p>
      <p>id {{edit_post.id}}</p>
      <p>title {{edit_post.title}}</p>
      <p>body {{edit_post.body}} </p>
      <button @click="editPost()">edit post</button>
    </div>
    <div v-if="edited">
      <p>id:
        <input v-model="temp_edit_post.id">
      </p>
      <p>title:
        <input v-model="temp_edit_post.title">
      </p>
      <p>body:
        <input v-model="temp_edit_post.body">
      </p>
      <button @click="change_post()">Change</button>
      <button @click="cancel_edit()">Cancel Edit</button>
    </div>
    <div>
      <button @click="back_post()">gotopost</button>
    </div>
  </div>
</template>
<script type="text/javascript">
export default {
  data: function() {
    return {
      temp_edit_post: {
        userId: "",
        id: "",
        title: "",
        body: ""
      },
      edit_post: [],
      edited: false,
    }
  },
  created: function() {
    var that = this;
    var post_data = window.localStorage.getItem("post");
    if (post_data == null) {
      post_data = JSON.stringify([]);
    }
    var post_array = JSON.parse(post_data);
    that.edit_post = post_array[that.$route.params.post_id];
  },
  methods: {
    back_post: function() {
      var that = this;
      var edit_data = window.localStorage.getItem("author_data");
      var edit_array = JSON.parse(edit_data);
      var index = edit_array.map(function(elem){
      	return elem.id
      }).indexOf(that.edit_post.userId);
      that.$router.push({ path: "/post/" + index})
    },
    editPost: function() {
      var that = this;
      that.edited = true;
      var post_data = window.localStorage.getItem("post")
      var post_array = JSON.parse(post_data)
      that.temp_edit_post = post_array[that.$route.params.post_id]
    },
    change_post: function() {
      var that = this;
      // var post_data = window.localStorage.getItem("post")
      // // // // Cho nay la xu ly bai post
      // // // var post_array = JSON.parse(post_data)
      // // // post_array[that.$route.params.post_id] = that.temp_edit_post
      // // // post_data = JSON.stringify(post_array);
      // // // window.localStorage.setItem("post", post_data)


      // // console.log(post_data);


      that.edit_post = that.temp_edit_post;
      // Chỗ này là để làm gì nhỉ Tun dge luu vao cai locastorage ma cho author_data cai post chi đê tam thoi
      var edit_data = window.localStorage.getItem("author_data");
      var edit_array = JSON.parse(edit_data);

      // Tim index cua user_id = that.edit_post.userId
      var index = edit_array.map(function(elem){
      	return elem.id
      }).indexOf(that.edit_post.userId)
      // Su dung index cua user_id da tim duoc

      edit_array[index].post[that.$route.params.post_id] = that.edit_post;
      edit_data = JSON.stringify(edit_array);
      window.localStorage.setItem("author_data", edit_data);

      // console.log(edit_array[that.edit_post.userId].post[that.$route.params.post_id])
      that.edited = false;
      // console.log(that.edit_post.userId) 
      // hieu :))
      // user_id khac voi index trong mang
      // Cho nay minh phai lam 1 buoc nua, do la tim duoc index trong mang cua user_id la that.edit_post.userId
      // Su dung map de tim index
      // ok hem? :D ok thank thay hay vai
      // ma mot van de nua: -> do not modify id lolz :v
      // id cua user hay id cua bai post la hem duoc modify
      // khi tao moi id cung nen auto tang len chu khong de user set manual
      // cac field id thuong dung de lam primary index, su dung cho quan he giua cac table voi nhau 
      // assume lai la hem cho user thay doi cac field nhu id nhen :)) roi oke hieu
      // chua them duoc phai hem? sao luc them dc luc ko them dc de xem lai da =))


      // *******************
      // minh xu ly cho lay index cua user
      // user_id != index cua user trong mang data
      // => minh phai co thuat toan de tim index cua user trong mang data, dua vao user_id
      // su dung ham map trong javascript array de lam viec nay
      // map -> custom lai mang vd: 1 mang object [{ userId : 'user_id-object1', name: 'name1' },{ userId : 'user_id-object2', name: 'name2' }] -> 1 mang ['user_id-object1','user_id-object2'] -> dung indexOf de tim ra vi tri cua user_id -> index cua user trong mang
      // roai do :) oke de mai sua lai cho no chuan :v
    },
    cancel_edit: function() {
      var that = this;
      that.edited = false;
    }
  }
}

</script>
<style>
sm-box {
  width: 300px;
  height: 300px;
}

.inform_pr {
  display: inline-block;
  text-align: center;
}

</style>
