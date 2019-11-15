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

## GET request with special relative path
Url: https://jsonplaceholder.typicode.com/posts/1/comments <br />
How to automate GET methods for different post number like (posts/2/comments) ?  <br />
Code: 
- use replacement keyword
- use that in the method with annotation to indicate the parameter
```
@GET("posts/{id}/comments")
Call<List<Comment>> getComments(@Path("id") int postId);
```
Calling this:
```
Call<List<Comment>> call = jsonPlaceHolderApi.getComments(3);
```

## GET request with parameters in the URL
https://jsonplaceholder.typicode.com/posts?userId=1&_sort=id&_order=desc <br />
```
    @GET("posts")
    Call<List<Post>> getUserPosts(@Query("userId") Integer[] userId,
                                  @Query("_sort") String sort,
                                  @Query("_order" ) String order);
```
Call this:
```
Call<List<Post>> call = jsonPlaceHolderApi.getUserPosts(new Integer[]{2, 3, 6}, "id", "desc");
```
## GET request with parameter as Map
https://jsonplaceholder.typicode.com/posts?userId=1&_sort=id&_order=desc <br />
```
 @GET("posts")
    Call<List<Post>> getUserPosts(@QueryMap Map<String, String> parameters);
```
Call this:
```
 Map<String, String> parameters = new HashMap<>();
        parameters.put("userId", "1");
        parameters.put("_sort", "id");
        parameters.put("_order", "desc");

```


### Resources
- https://codinginflow.com/tutorials/android/retrofit/part-1-simple-get-request
