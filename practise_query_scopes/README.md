<h1 style="position:relative; top: -6px" > 
:question: Practise Eloquent Local Query Scopes
</h1>

<h3>
**This article is only about local scopes if you want to use global scopes check this link below**
</h3>

Link: https://www.itsolutionstuff.com/post/laravel-global-scope-tutorial-exampleexample.html

---
<br>
<br>
Using scopes in Laravel makes your code more readable and maintainable by encapsulating common query logic in methods that can be easily reused throughout your application. Here's an example that illustrates the benefits of using scopes.


<h3>Scenario: Blog Application</h3>
Imagine you're building a blog application where you often need to filter posts based on their status, such as "published", "draft", or "archived". Without scopes, you might find yourself repeatedly writing the same filtering logic in various parts of your application, leading to code duplication and a higher chance of errors.

<br>
<h2>Without Scopes</h2>

Here's how you might write queries without using scopes:

```php

// Fetching published posts
$publishedPosts = Post::where('status', 'published')->get();

// Fetching draft posts
$draftPosts = Post::where('status', 'draft')->get();

// Fetching archived posts
$archivedPosts = Post::where('status', 'archived')->get();

```

<h2>With Scopes</h2>

Now, let's define some scopes in the Post model to encapsulate the filtering logic for different post statuses:

```php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    // Other model properties and methods...

    public function scopePublished($query)
    {
        return $query->where('status', 'published');
    }

    public function scopeDraft($query)
    {
        return $query->where('status', 'draft');
    }

    public function scopeArchived($query)
    {
        return $query->where('status', 'archived');
    }
}

```
Using these scopes, your queries become more expressive and easier to read:

```php

// Fetching published posts
$publishedPosts = Post::published()->get();

// Fetching draft posts
$draftPosts = Post::draft()->get();

// Fetching archived posts
$archivedPosts = Post::archived()->get();

```

<br>

<h2>Make the Scope Dynamic by Adding Parameters</h2>

```php

public function scopeStatus($query, $type)
    {
        return $query->where('status', $type);
    }

```

Use in controller:
```php
Post::status(1)->today()->get();
```



<br>

<h2>😄Benefits of Using Scopes</h2>
1.DRY Principle: You adhere to the "Don't Repeat Yourself" principle by encapsulating common query logic within the model. This reduces the risk of errors that can occur when copying and pasting the same logic in multiple places.
<br>

2.Readability: Scopes make your code more expressive. Reading 

```php
Post::published()->get();
```
 clearly conveys what the code does without having to dig into the specifics of the query.
 <br>
 3.Reusability: Once defined, scopes can be reused across your application, making your code more modular and easier to maintain.
 <br>
 4.Chainability: Scopes can be chained with other query builder methods to construct more complex queries while keeping the code clean and readable.
 <br>
 5.Customization: Scopes can accept parameters, giving them the flexibility to cover a wide range of query scenarios beyond simple static conditions.