# Node Bing API
Node.js lib for the Azure Bing Web Search API

## Changelog v2
In order to follow JavaScript best practices and allow the library to
be promisified, the callback function is now the last parameter.

## Installation
````
npm install node-bing-api
````

## Usage

Require the library and initialialize it with your account key:

```js
var Bing = require('node-bing-api')({ accKey: "your-account-key" });
```

#### Web Search:
```js
Bing.web("Pizza", {
    top: 10,  // Number of results (max 50)
    skip: 3   // Skip first 3 results
  }, function(error, res, body){

    // body has more useful information, but for this example we are just
    // printing the first two results
    console.log(body.webPages.value[0]);
    console.log(body.webPages.value[1]);
  });
```

#### Composite Search:
```js
Bing.composite("xbox", {
    top: 10,  // Number of results (max 15 for news, max 50 if other)
    skip: 3   // Skip first 3 results
  }, function(error, res, body){
    console.log(body);
  });
```

#### News Search:
```js
Bing.news("xbox", {
    top: 10,  // Number of results (max 15)
    skip: 3   // Skip first 3 results
  }, function(error, res, body){
    console.log(body);
  });
```

#### Video Search:
```js
Bing.video("monkey vs frog", {
    top: 10,  // Number of results (max 50)
    skip: 3   // Skip first 3 result
  }, function(error, res, body){
    console.log(body);
  });
```

#### Images Search:
```js
Bing.images("Ninja Turtles", {skip: 50}, function(error, res, body){
  console.log(body);
});
```
Adding filter(s) for the Image Search
```js
Bing.images("Ninja Turtles", {
    top: 5  // Number of results (max 50)
  }, function(error, res, body){
    console.log(body);
  });
```

#### Related Search:
```js
Bing.relatedSearch('berlin', {market: 'en-US'}, function (err, res, body) {
  var suggestions = body.relatedSearches.value.map(function(r){ return r.Title; });
  console.log(suggestions.join('\n'));
});
```

#### Spelling Suggestions:
```js
Bing.spelling('awsome spell', function (err, res, body) {
  console.log(body.flaggedTokens.suggestions[0].suggestion); //awesome spell
});
```

Requires specific Account key

Available Options:
* mode:\<Proof | Spell\>
* preContextText:\<*String*\>
* postContextText:\<*String*\>

#### Specify Market
Getting spanish results:
```js
Bing.images("Ninja Turtles"
          , {top: 5, market: 'es-ES'}
          , function(error, res, body){

  console.log(body);
});
```
[List of Bing Markets](https://msdn.microsoft.com/en-us/library/dd251064.aspx)


#### Adult Filter
```js
Bing.images('Kim Kardashian'
          , {market: 'en-US', adult: 'Strict'}
          , function(error, res, body){

  console.log(body.d.results);
});
```
Accepted values: "Off", "Moderate", "Strict".

*Moderate level should not include results with sexually explicit images
or videos, but may include sexually explicit text.*

#### Web Only API Subscriptions
To use this library with a web only subscription, you can require and initialize it with an alternate root url:
```js
var Bing = require('node-bing-api')
            ({
              accKey: "your-account-key",
              rootUri: "https://api.datamarket.azure.com/Bing/SearchWeb/v1/"
            });
```

## Running Tests
In order to run the tests, the integration tests require to create a `secrets.js` file
from the provided `secrets.js.example` example, and fill it in with a valid access key.

Then just `mocha`.


## License
MIT
