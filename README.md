![Version](https://img.shields.io/badge/Version-0.3.1-blue.svg)
![MinGhostVersion](https://img.shields.io/badge/Min%20Ghost%20v.-%3E%3D%200.7.x-red.svg)

## 写在前面

这个项目是我 fork 来的，根据我的博客实际需求对其代码内容稍微改造，原地址在这：[ghostHunter](https://github.com/jamalneufeld/ghostHunter).

我改造的内容大致如下(改造文件在 song 文件夹下面):

1. 针对 Ghost 博客提供给使用者的 api，在增加搜索功能的同时，只接受 description、tag、id 三个参数，我觉得其他参数对于我的博客来说没用。改造后的 [demo](http://blog.songjz.cn) .
2. 对于 Hexo 博客，变动稍大，因为原先的接口都变了，所以只能手动通过 JQuery 来传递 XML 文件并且解析 XML 文件，数据风格和 Ghost 一样，弊端就是 需要 Hexo 的搜索插件配合，即需要改变 Hexo 的 Search 插件。改造后的 [demo](http://yuren.space/blog).

**This tool has just  been updated to work with Ghost's API. To quote the Ghost team, "The API is still under very (very) heavy development and subject to regular breaking changes."**

**If performance is an issue, you should probably remove the "markdown" field from the index and the API query.**

#Updated
Searching through tags is now supported. Special thanks to @lizhuoli1126 for the help.
Added the prettypubdate as coded by @alavers

#ToDo
- Restrict the number of fields being queried from the API.
	~~Status @05/14/2016 : the bug has been closed by the Ghost team. Anyone downloading Ghost from Git can use the "filterfields" branch of this fork. All others : wait for the ghost release O.8.0~~
	Status @26/05/2016 : the bug has been reopened on the Ghost end. Waiting for resolution of bug #6625 @TryGhost.

- Add the possibility to build the index on the server at a set time, and query that index instead of building it every time the page loads
- Allow switching between building the index live and the aforementioned function.
- In the distant future, build a GUI to set which fields can be queried, how much to boost their importance, etc.

#GhostHunter
A Ghost blog search engine

==============

GhostHunter allows any theme developer to add search capabilities right in their blog without having to resort to any third-party solutions. 

GhostHunter has at its core [lunr.js](http://lunrjs.com). And thanks to this powerful search engine, GhostHunter provides full text searching.

original developer : jamal@i11u.me

##Usage

After including jQuery, include ghostHunter:

````html
    <script src="js/jquery.ghostHunter.min.js"></script>
````

In your theme you'll need an `<input>` for the search query, the input should be in a `<form>` and you can use the form's submit functionality to call the search. This will work whether you have a standard submit button or if you use submit() in javascript.
````html
    <form>
      <input id="search-field" />
      <input type="submit" value="search">
    </form>
````
You'll also need an area for the search results to show up:
````html
    <section id="results"></section>
````
Now we can turn on the plugin, and that's all there is to it:
````js
    $("#search-field").ghostHunter({
      results   : "#results"
    });
````
##How it works

GhostHunter will attach itself to your search input field and wait for it to be focused on. Once focus has gone to the field, the engine quickly gets to work building an index of your ghost posts that can easily be searched. When your visitor submits the form, GhostHunter will use either a default template or one provided by you to fill in your results element.

##Advanced features

GhostHunter can be customized to a certain extent using very simple templating. 

If you'd like to customize the html of the results there are two options:

###Having search results appear on key up

You can have the search results appear "as you type". Simply pass the onKeyUp parameter as true
````js
	$("#search-field").ghostHunter({
		results   		: "#results",
		onKeyUp 		: true
    });
````
###Adding callbacks

You can have Ghost Hunter call your callback function at two points. The first is right before it renders the information onto the page using the "before" option:
````js
	$("#search-field").ghostHunter({
		results   		: "#results",
		before 			: function(){
			alert("results are about to be rendered");
		}
    });
````
The other callback is "onComplete" and gets called when the results have been rendered. It also provides an array of all the results:
````js
	$("#search-field").ghostHunter({
		results   		: "#results",
		onComplete		: function( results ){
			console.log( results );
			alert("results have been rendered");
		}
    });
````

###Clearing the results

You can use ghostHunter to clear the results of your query. ghostHunter will return an object relating to your search field and you can use that object to clear results.
````js
	var searchField = 	$("#search-field").ghostHunter({
							results   		: "#results",
							onKeyUp 		: true
					    });
````
Now that the object is available to your code you can call it any time to clear your results:
````js
	searchField.clear();
````
###Hiding the search info

If you don't want to show the search info at all you can pass this option
````js
	$("#search-field").ghostHunter({
		results   			: "#results",
		displaySearchInfo 	: false
    });
````
###Hiding the search info when the result is zero

If you don't want to show the search info when the results are zero you can pass this option
````js
	$("#search-field").ghostHunter({
		results   			: "#results",
		zeroResultsInfo 	: false
    });
````
###Customizing the html template

The **result template** has access to these variables: title, description, link, pubDate.

If you'd like to create your own you can use double curly brackets and pass the "results_template" option:
````js
	$("#search-field").ghostHunter({
		results   		: "#results",
		result_template : "<a href='{{link}}'><p><h2>{{title}}</h2><h4>{{pubDate}}</h4>{{description}}</p></a>"
    });
````
The **info template** has one variable: amount.

If you would like to modify the wording at the top, or have it say nothing you'll need to pass in the "info_template":
````js
	$("#search-field").ghostHunter({
		results   		: "#results",
		info_template	: "<p>Number of posts found: {{amount}}</p>"
    });
````
And of course, both can be included together:
````js
	$("#search-field").ghostHunter({
		results   		: "#results",
		info_template	: "<p>Number of posts found: {{amount}}</p>",
		result_template : "<a href='{{link}}'><p><h2>{{title}}</h2><h4>{{pubDate}}</h4>{{description}}</p></a>"
    });
````
