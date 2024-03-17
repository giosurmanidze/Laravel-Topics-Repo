<h1 style="position:relative; top: -6px" > 
:question: Mutators, Casting and Accessors
</h1>


Accessors in Laravel enable you to modify the data fetched from the database before it’s accessed by your application’s code. This is particularly useful when you want to present data in a specific format without modifying the actual stored value.


Define the Accessor Inside your model, define an accessor method using the following naming convention: get{AttributeName}Attribute

```php

    public function getFullNameAttribute()
    {
        return $this->attributes['first_name'] . ' ' . $this->attributes['last_name'];
    }

```


Mutators, on the other hand, allow you to manipulate data before it’s stored in the database. This is useful for performing transformations or validations on data before persisting it.


Define the Mutator Define a mutator method using the naming convention: set{AttributeName}Attribute

```php

Example 1:

    public function setTitleAttribute($value)
    {
        $this->attributes['title'] = strtoupper($value);
    }

Example 2:
    public function setPasswordAttribute($value)
    {
        $this->attributes['password'] = bcrypt($value);
    }
```


