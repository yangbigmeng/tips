# 下载大文件导致内存压力过大
  
>   参考 https://www.baeldung.com/spring-resttemplate-download-large-file
```java
// 反例
Resource download() {
    return new ClassPathResource(locationForLargeFile);
}
```
这不能工作的原因是ResourceHttpMesssageConverter会将整个响应体加载到ByteArrayInputStream中，仍然会增加我们想要避免的内存压力。

**解决方法**：使用创建RestTemplate。使用自定义ResponseExtractor执行，将输入流存储在文件中
```java
File file = restTemplate.execute(FILE_URL, HttpMethod.GET, null, new ResponseExtractor<File>() {
	@Override
	public File extractData(ClientHttpResponse response) throw IOException {
		File ret = File.createTempFile("download", "tmp");
        StreamUtils.copy(clientHttpResponse.getBody(), new FileOutputStream(ret));
        return ret;
	}
});
Assert.assertNotNull(file);
```
