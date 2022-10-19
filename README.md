### Laravel-Notes
### Lumen
Generate project of Lumen
```
Generate project for lumen
$composer create-project --prefer-dist laravel/lumen blog-app
```
Generate app key for .env using PHP interactive mode
```
$php -a
echo substr(md5(rand()),0,32); //32 chars
```

### Eager Loading

```
//User
public post function(){
    return $this->hasMany(PostClass::class);
}
//Post
public userpost function(){
    return $this->belongsto(UserClass::class);
}

//Schema User
id
name
//Schema Post
id
user_id
post
```
### References

[Sample Eagerloading](https://vegibit.com/laravel-hasmany-and-belongsto-tutorial/)
