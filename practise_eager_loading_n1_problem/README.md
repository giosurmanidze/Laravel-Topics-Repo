<h1 style="position:relative; top: -6px" > 
:question: Practise Eager Loading And N+1 Problem
</h1>

<h3>
**Before we get started, check out this helpful link
Laravel N+1 issue detector to help you easily identify n+1 issues in your application**
</h3>
Link: https://github.com/beyondcode/laravel-query-detector


<br>
<br>
<h3>:question: Task1 Scenario:</h3>
Suppose we have two Eloquent models: ` Post `  and ` Comment `. Each post can have multiple comments associated with it, and we want to display a list of posts along with their comments.

<br>
<h3>Objective:</h3>
1.Fetch all posts from the database.
<br>
2.Loop through each post and retrieve its associated comments.
<br>
3.Display the post title and the number of comments for each post.

<br>
<h3>Code Example:</h3>

```php

// Fetch all posts from the database
$posts = Post::all();

// Loop through each post and retrieve its comments
foreach ($posts as $post) {
    // Retrieve comments associated with the current post
    $comments = $post->comments;

    // Output the post title and the number of comments
    echo "Post Title: $post->title (Comments: " . count($comments) . ")\n";
}

```

<br>
<h3>Problem:</h3>
In the above example, for each post, a separate query is executed to retrieve its associated comments. This leads to the N+1 query problem, where there's one initial query to fetch posts and an additional query for each post to retrieve its comments individually.

<br>
<h3>Fix:</h3>
To mitigate the N+1 query problem, we can use eager loading to retrieve posts along with their comments in a single query.
<h3>Updated Code Example with Eager Loading:</h3>

```php

// Fetch all posts with their comments using eager loading
$posts = Post::with('comments')->get();

// Loop through each post and retrieve its comments
foreach ($posts as $post) {
    // Retrieve comments associated with the current post
    $comments = $post->comments;

    // Output the post title and the number of comments
    echo "Post Title: $post->title (Comments: " . count($comments) . ")\n";
}

```

<br>
<h3>Explanation:</h3>
In the updated code, we use the with('comments') method to eager load the comments relationship along with the posts. This results in only two queries: one to fetch all posts and another to fetch all associated comments. As a result, we eliminate the N+1 query problem and improve the efficiency of our code.

<br>
<br>
<br>
<h3>:question: Task2 Scenario:</h3>
Let's say that you have the hasMany relationship between posts and comments, and you need to list the posts with the number of comments for each of them.

<br>
<h3>Code Example:</h3>

Controller code could be:

```php

public function index()
{
    $posts = Post::with('comments')->get();

    return view('posts.index', compact('posts'));
}

```

And then, in the Blade file, you do a foreach loop for the table:

```php
@foreach($posts as $post)
    <tr>
        <td>{{ $post->name }}</td>
        <td>{{ $post->comments()->count() }}</td>
    </tr>
@endforeach
```

Looks good, right 😊? And it works. But look at the Debugbar data.
Install Debugbar From: https://github.com/barryvdh/laravel-debugbar

But wait 😩, you would say that we are using eager loading, Post::with('comments'), so why there are so many queries happening?
<br>
**What do you think what is issue here and how to fix it?**

<br>
<h3>Explanation:</h3

So Because, in Blade,

```php
  $post->comments()->count()
```

doesn't actually load that relationship from the memory.

```php
$post->comments() means the METHOD of relation
```

```php
$post->comments means the DATA eager loaded into memory
```

So, the method of relation would query the database for each post. But if you load the data, without () symbols, it will successfully use the eager loaded data:

<h3>Updated Code Example:</h3>

Controller code:

```php

public function index()
{
    $posts = Post::withCount('comments')->get();

    return view('posts.index', compact('posts'));
}

```

Blade code:

```php

@foreach($posts as $post)
    <tr>
        <td>{{ $post->name }}</td>
        <td>{{ $post->comments_count }}</td>
    </tr>
@endforeach

```
