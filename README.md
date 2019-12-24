## Why?

I found many jQuery Twitter Plugins, but most of them work with searching and displaying the tweets. In the project that I worked the layout to display the tweets was specific. So I couldn't use the plugins' layout.

The main idea of Twitter Search Plugin is only for tweets search. You will need to implement the layout to display the tweets.

## What's the difference 

The available plugins on the internet use an ugly syntax to search:

```javascript
  $.twitter.search({from: 'pablocantero', text: 'hello+world', container: 'myDivId'});
```

### Twitter Search Plugin is DSL based

```javascript
  var tweets = new Twitter.tweets();
  tweets.containing('hello')
	.and('world')
	.from('pablocantero)
	.limit(4)
	.order('recent')
	.all(function(data){ 
		alert("It’s a customized function to display the tweets");
	});

```

## The Twitter Search API

This Plugin follows the Twitter Search API documentation.

Twitter Search API Method: search
http://apiwiki.twitter.com/w/page/22554756/Twitter-Search-API-Method:-search

### Implemented conditions 

The actual conditions were implemented on-demand. If you want a new condition, please have a look at "Do you want to improve the jQuery Twitter Plugin?".

 * conditions([text or hashtag])
 * and([text or hashtag])
 * or([text or hashtag])
 * from([twitter username])
 * to([twitter username])
 * order([mixed, recent or popular]) - default recent
 * limit([0..100]) - default 100
 * page([page num]) - default 1 
 * locale([locale]) - optional. specify the language of the query you are sending (only ja is currently effective). This is intended for language-specific clients and the default should work in the majority of cases

## Example

The full example is available on https://gist.github.com/758666

### Create success callback

This plugin is only for tweets search, it does not display the tweets. You need to implement the layout to display the tweets.

```javascript

  /*
  Customized callback to show the tweets
  */
  var tweetsSuccessCallback = function(data){
  	var tweetsLi = '';
  	for(var i = 0; i < data.results.length; i++){
  		var tweet = data.results[i];
  		// Calculate how many hours ago was the tweet posted
  		// http://www.lupomontero.com/fetching-tweets-with-jquery-and-the-twitter-json-api/
  		var dateTweet = new Date(tweet.created_at);
  		var dateNow   = new Date();
  		var dateDiff  = dateNow - dateTweet;
  		var hours     = Math.round(dateDiff/(1000*60*60));
  		tweetsLi += '<li> \
  		      <dl> \
  		              <dt><img src="' + tweet.profile_image_url + '" /></dt> \
  		              <dd> \
  		                      <span><a href="http://twitter.com/' + tweet.from_user + '">' + tweet.from_user + '</a></span> \
  		                      <span>' + hours + ' hours ago</span></dd></dl> \
  		      <div>' + tweet.text + '</div> \
  		</li>';
  	}
  	$('#tweets').html('<ul>' + tweetsLi + '</ul>');
  }
```

### Create an error callback

Customized callback to display an error message
This callback is optional, the default error callback shows an alert(errorMessage).

```javascript
  var tweetsErrorCallback = function(errorMessage, tweet){
	var msg = 'Oops! \
		<a href="http://dev.twitter.com/pages/rate-limiting">Twitter Rate Limit Exceeded</a>. \
		Try again later or search \
		<a href="http://twitter.com/search?q=' + tweet.toQuery() + '">directly on Twitter</a>';
	$('#tweets').html(msg);
  }
```

### Create a search query

```javascript
  var tweets = Twitter.tweets();
  tweets.containing('jquery.twitter').all(tweetsSuccessCallback, tweetsErrorCallback);
```

## Combine OR and FROM queries

Thanks to pheze issue/2.

You can combine OR and FROM queries.

```javascript
  var tweets = new Twitter.tweets();
  tweets.containing('jquery-twitter')
	.or()
	.from('pablocantero')
	.all(function(data){ 
	});
```

This query above return all tweets containing 'jquery-twitter', if the result is empty it will return all tweets from @pablocantero

## Test

To execute the Test suite, just open jquery.twitter-test.html in your browser. The tests were developed with http://docs.jquery.com/Qunit

## Do you want to improve the jQuery Twitter Plugin?

You’re welcome to make your contributions and send them as a pull request or just send me a message http://pablocantero.com/blog/contato/

http://pablocantero.com/blog/2010/12/29/jquery-twitter-search-plugin/

http://plugins.jquery.com/project/jquery-twitter

### Contributors

Thanks to pheze
