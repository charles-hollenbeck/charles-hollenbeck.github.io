---
layout: post
title:  "Nesting PHP Classes"
date:   2014-02-08
categories: php
---

So a buddy of mine and I were trying to figure out how to make our PHP class for connecting to our MongoDB do something like:

{% highlight php %}
<?
$database = new Database();
$database->posts->add("Hello world!");
?>
{% endhighlight %}

and we found out that you can't nest classes with PHP like you can in some other [programming languages][oracle]. Our solution was to create a class with public variables that would hold the other classes.

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

This tactic for nesting PHP classes really helped clean up a messy garble of code and provide a more readable format than what we were using previously.

[oracle]: http://docs.oracle.com/javase/tutorial/java/javaOO/nested.html