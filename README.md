# how-to-upload-multiple-files-using-retrofit-in-android
Uploading multiple files in using Retrofit in android


### Service interface
```
public interface ApiService {

    @POST("Controller/Action")
    Call<ResponseBody> postMeme(@Body RequestBody files);
}

``` 

### Client class
```
public class ApiClient   {
    public static final String API_BASE_URL = "api base url";

    private static OkHttpClient.Builder httpClient = new OkHttpClient.Builder();

    private static Retrofit.Builder builder = new Retrofit.Builder().baseUrl(API_BASE_URL).addConverterFactory(GsonConverterFactory.create());

    public static ApiService createService(Class<ApiService> serviceClass)
    {
        Retrofit retrofit = builder.client(httpClient.build()).build();
        return retrofit.create(serviceClass);
    }
}
```


### Uploading file in the activity/ fragment class
```
    ApiService service = ApiClient.createService(ApiService.class);
        
    MultipartBody.Builder builder = new MultipartBody.Builder();
    builder.setType(MultipartBody.FORM);
    builder.addFormDataPart("files",file1.getName(),RequestBody.create(MediaType.parse("image/*"), file1));
    builder.addFormDataPart("files",file2.getName(),RequestBody.create(MediaType.parse("image/*"), file2));


    MultipartBody requestBody = builder.build();
    Call<ResponseBody> call = service.postMeme(requestBody);
    call.enqueue(new Callback<ResponseBody>() {
         @Override
         public void onResponse(Call<ResponseBody> call, Response<ResponseBody> response) {
             Toast.makeText(getBaseContext(),"All fine",Toast.LENGTH_SHORT).show();
         }

         @Override
         public void onFailure(Call<ResponseBody> call, Throwable t) {
            Toast.makeText(getBaseContext(),t.getMessage(),Toast.LENGTH_SHORT).show();
         }
     });
```

### Handling request on the server-side (ASP.NET MVC)
```
[HttpPost]
public JsonResult CreateMemePost(IEnumerable<HttpPostedFileBase> files)
{
     //Do something with files
}
```
