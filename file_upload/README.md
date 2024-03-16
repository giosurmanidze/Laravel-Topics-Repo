<h1 style="position:relative; top: -6px" > 
:question: File Uploads
</h1>

<h3>Here is some code example:</h3>

```php

public function create($data)
    {
        $attributes = [
            'title' => $data['title'],
            'content' => $data['content'],
            'status' => $data['status']
        ];

        $post = Post::create($attributes);

        if(request()->hasFile('image')) {
            $image = request()->file('image');
            $imageName = time().'_'.$image->getClientOriginalName(); // Create a unique name for the file
    
            // Attempt to store the file in the 'images' disk (folder)
            $image->storeAs('images', $imageName, 'public'); // Assuming you're using the 'public' disk
    
            // Add 'image' to the attributes array with the path of the stored image
            $attributes['image'] = $imageName;

           $post->image()->create([
            'image' => $attributes['image']
        ]);
        };
        return $post;
    }

```