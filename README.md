# [Android-Retrofit](https://github.com/square/retrofit)
Basic android app demonstrating Retrofit library <br />

### Installation
```
implementation 'com.squareup.retrofit2:converter-gson:2.6.2'
implementation 'com.squareup.retrofit2:retrofit:2.6.2'
```

## [GET request](https://jsonplaceholder.typicode.com/posts)
URL: https://jsonplaceholder.typicode.com/posts
Sample output:
```
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
  },
  {
    "userId": 1,
    "id": 2,
    "title": "qui est esse",
    "body": "est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla"
  },
  {
    "userId": 1,
    "id": 3,
    "title": "ea molestias quasi exercitationem repellat qui ipsa sit aut",
    "body": "et iusto sed quo iure\nvoluptatem occaecati omnis eligendi aut ad\nvoluptatem doloribus vel accusantium quis pariatur\nmolestiae porro eius odio et labore et velit aut"
  }  
]
```
### Steps
- Create a model class `Post.java` representing the object of JSON.
- Each field name in the file must correspond to the field name of the JSON object. 
- You can use placeholder field name linking to real field using annotation.
Example:
```
    @SerializedName("body")
    private  String text;
```
Here `body` is the field inside JSON data. `text` field name is used in the program. 
- Create an interface `JsonPlaceHolderApi`
```
public interface JsonPlaceHolderApi {

    @GET("posts")
    Call<List<Post>> getPosts();
}

```
- We are getting List of Posts from the JSON, hence List<Post> inside Call
- `posts` is the relative url 
### Creating the GET request
```
textViewResult = findViewById(R.id.text_view_result);

Retrofit retrofit = new Retrofit.Builder()
        .baseUrl("https://jsonplaceholder.typicode.com/")
        .addConverterFactory(GsonConverterFactory.create())
        .build();

JsonPlaceHolderApi jsonPlaceHolderApi = retrofit.create(JsonPlaceHolderApi.class);

Call<List<Post>> call = jsonPlaceHolderApi.getPosts();

call.enqueue(new Callback<List<Post>>() {
    @Override
    public void onResponse(Call<List<Post>> call, Response<List<Post>> response) {

        if(!response.isSuccessful()) {
            textViewResult.setText("Code " + response.code());
            return;
        }

        List<Post> posts = response.body();
        for(Post post: posts) {
            String content = "";
            content += "ID: " + post.getId() + "\n";
            content += "User ID: " + post.getUserId() + "\n";
            content += "Title: " + post.getTitle() + "\n";
            content += "Text: " + post.getText() + "\n\n";

            textViewResult.append(content);
        }

    }

    @Override
    public void onFailure(Call<List<Post>> call, Throwable t) {
        textViewResult.setText(t.getMessage());
    }
});

```

### Resources
- https://codinginflow.com/tutorials/android/retrofit/part-1-simple-get-request
