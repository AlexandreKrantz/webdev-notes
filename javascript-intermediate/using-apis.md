# Working with APIs
- Def. API = server created for serving data for external use in websites or apps.
- APIs are accessed through URLs (in most cases). Different URLs under the same domain correspond to different specific data requests. 
- In most cases, you will have to get an API key to use an API service. The key is included in your final URL used to make the request.
  - A good practice is to store your private API keys on the server and never send them to the frontend in the first place, this is often done using environment variables and it makes the key available only on the server the code is deployed to.
- You basically append the variables to the end of the API call URL: `apicallurl.com/dosomthing?api_key=YOUR_KEY_HERE&param=SOMTHING&anotherparam=SOMETHING_ELSE`
- The old way to access API data in your code was using `XMLHttpRequest`, but now the newer (and better) way is using `fetch`. Fetch makes uses of promises:

```javascript
// params: URL (required), options (optional)
fetch('https://url.com/some/url')
  .then(function(response) {
    // Successful response :)
  })
  .catch(function(err) {
    // Error :(
  });
```
- By default, browsers restrict HTTP requests to outside sources for security reasons. Cross Origin Resource Sharing (CORS) is the fix: 
```javascript
fetch('url.url.com/api', {
  mode: 'cors'
});
```

## Details of Fetch
- `fetch()` is a global method. Fetch returns a promise that is always fufilled unless the network failed and the request wasn't made (in which case it rejects). If the request results in an HTTP error (eg. 404), then the `ok` property of the response is set to false.
- Fetch does not directly return the JSON response body but instead returns a promise that resolves with a Response object. It is a representation of the entire HTTP response, not just the data that was returned. 
- So, to extract the JSON body content from the Response object, we use the `.json()` method, which returns a second promise that resolves with the result of parsing the response body text as JSON.
```javascript
fetch('http://example.com/movies.json')
  .then(response => response.json())
  .then(data => console.log(data));
```
