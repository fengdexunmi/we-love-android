# we-love-android
Android Basics
# 阅读OkHttp源码
> OkHttp--An Http & Http/2 client for Android and Java applications
## OkHttp的基本使用
使用创建一个OkHttpClient，然后构建一个request对象，再然后发送这个请求，可以是同步请求，也可以是异步请求
```Java
OkHttpClient client = new OkHttpClient();
Request request = new Request.Builder().url(ENDPOINT).build();

try {
    // 同步请求
    Response response = client.newCall(request).execute();
} catch (IOException e) {
    e.printStackTrace();
}

// 异步请求
client.newCall(request).enqueue(new Callback() {
    @Override
    public void onFailure(Call call, IOException e) {

    }

    @Override
    public void onResponse(Call call, Response response) throws IOException {

    }
});
```
## OkHttp调用流程