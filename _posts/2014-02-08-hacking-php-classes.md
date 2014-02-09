---
layout: post
title:  "Hacking PHP Classes"
date:   2014-02-08
categories: php
---

So a buddy of mine and me were trying to figure out how to make our PHP class for connecting to our MongoDB do something like:

{% highlight php %}
<?
$database = new Database();
$database->posts->add("Hello world!");
?>
{% endhighlight %}

and we found out that you can't nest classes with php like you can in some other [programming languages][oracle]. So the solution was to create a class with public variables that would hold the other classes.

**Example:**
{% highlight php %}
<?
class Database {
	private $db;
	public $posts;

	public function __construct() {
		$mongo = new Mongo();
		$this->db = $mongo->XD6;
		$this->posts = new Posts($this->db);
	}
}
?>
{% endhighlight %}

Therefore now we can do something in the posts class like:
{% highlight php %}
<?
class Posts{
	private $db;

	public function __construct($db) {
		$this->db = $db;
	}

	public function add($title) {
		$posts->insert(array("title" => $title));
	}
}
?>
{% endhighlight %}

and then be able to call it in a nicer fashion like this:
{% highlight php %}
<?
$database = new Database();
$database->posts->add("Hello World!");
?>
{% endhighlight %}

Rather than something like this:
{% highlight php %}
<?
$database = new Database();
$database->addPost("Hello World!");
?>
{% endhighlight %}

This tactic for nesting PHP classes that my friend and I used really helped clean up a messing looking garble of code and provide more readable format than what we were using.

[oracle]: http://docs.oracle.com/javase/tutorial/java/javaOO/nested.html