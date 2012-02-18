Chosen ajax addition
====================
We use [chosen](https://github.com/harvesthq/chosen) at work.
Works great, but one day we needed it to do some server side calls.
So I quickly wrapped the plugin with some jQuery magic and chosen ajax addition was born.
Although it doesn't fit with the progressive enhancement ideals of chosen.. it was born quickly and it does what we need it to do for the time being.

Warning
-------
Known issues:

* This plugin is known to do double work. If the user types a few letters, pauses, ajax fires off and returns results. Then the user types more, the chosen plugin will filter the results via JS as well as the server from the ajax call. 
* User types, ajax 1 fires, users deletes, ajax 1 finishes and ajax 2 fires off. This sometimes leaves the input in an inconsistent state.

Example Usage
-------------
```javascript
<script src="/js/vendor/jquery-1.7.1.min.js"></script>
<script src="/js/vendor/chosen.jquery-0.9.5.js"></script>
<script src="/js/chosen.ajaxaddition.jquery.js"></script>
<script>
	$('select').ajaxChosen(ajaxOptions, ajaxChosenOptions, chosenOptions);
</script>
```
The method signature is:
ajaxOptions - options for $.ajax
ajaxChosenOptions - options for the ajax chosen plugin
chosenOptions - options to be proxied to the chosen plugin. ex: {no_results_text: 'shit out of luck'}

```javascript
<script src="/js/vendor/jquery-1.7.1.min.js"></script>
<script src="/js/vendor/chosen.jquery-0.9.5.js"></script>
<script src="/js/chosen.ajaxaddition.jquery.js"></script>
<script>
$('select').ajaxChosen({
	dataType: 'json',
	type: 'POST',
	url:'/search',
	data: {'keyboard':'cat'} //Or can be [{'name':'keyboard', 'value':'cat'}]. chose your favorite, it handles both.
	success: function(data, textStatus, jqXHR){ doSomething(); }
},{
	processItems: function(data){ return data.complex.results; }
	useAjax: function(e){ return someCheckboxIsChecked(); },
	generateUrl: function(){ return '/search_page/'+somethingDynamical(); },
	loadingImg: '../vendor/loading.gif'
});
</script>
```

	processItems -> this function gets called on the data that gets returned from the server, so you can format your results before ajax chosen outputs the select options. It is expected to return an array of key-value pairs or a hash of key value pairs. See below for details.
	
	default: nothing

	useAjax -> this function will be executed on key up to determine whether to use the ajax functionality or not. It must return true or false.
	
	default: true

	generateUrl -> this function will get executed right before the ajax call is fired. It will use the return value of this function as the url option for the ajax call.
	
	default: nothing, uses the url specified in the ajax parameters

	loadingImg -> path to the image you wish to show when the ajax call is processing
	
	default: '/img/loading.gif'


Expected Data Formats
---------------------
Ajax chosen requires the server return data to be in a somewhat specific format for it to output the select options.

It can be an array of kv pairs:
```javascript
	[{id:'', text:''}...]
```

It can be a hash of kv pairs
```javascript
	{'id':'text'...}
```

Another thing it expects is the query to be passed back in the result set. For instance:

```javascript
	{q:'banana', results:[{id:'', text:''}]}
```


Which brings me to my next point. If no processItems function is specified, ajax chosen will search for a results key in the data hash. If it is not found it will give you an error.
So some example formats that will work without a processItems function:

```javascript
	{q:'banana', results:[{id:'', text:''}]}
	{q:'banana', results:{id:text,id:text...}}
```


Running the tests
-----------------
Just load up runner.html in your browser

Vendor
------
[jquery](http://jquery.com/)

[chosen](https://github.com/harvesthq/chosen) - base plugin

[mocha](http://visionmedia.github.com/mocha/) - test framework

[chai](http://chaijs.com/) - browser expectations for mocha

[sinon](http://sinonjs.org/) - spies, stubs, and mocks

[json2](https://github.com/douglascrockford/JSON-js) - JSON.stringify


License
-------
Standard MIT

Copyright (c) 2012 Konstantin Sykulev

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.