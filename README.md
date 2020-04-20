Laravel Favorite

---

❤️ User favorite feature for Laravel Application.

![CI](https://github.com/overtrue/laravel-favorite/workflows/CI/badge.svg)


## Installing

```shell
$ composer require overtrue/laravel-favorite -vvv
```

### Configuration

This step is optional

```php
$ php artisan vendor:publish --provider="Overtrue\\LaravelFavorite\\FavoriteServiceProvider" --tag=config
```

### Migrations

This step is also optional, if you want to custom favorites table, you can publish the migration files:

```php
$ php artisan vendor:publish --provider="Overtrue\\LaravelFavorite\\FavoriteServiceProvider" --tag=migrations
```


## Usage

### Traits

#### `Overtrue\LaravelFavorite\Traits\Favoriter`

```php

use Illuminate\Notifications\Notifiable;
use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Overtrue\LaravelFavorite\Traits\Favoriter;

class User extends Authenticatable
{
    use Favoriter;
    
    <...>
}
```

#### `Overtrue\LaravelFavorite\Traits\Favoriteable`

```php
use Illuminate\Database\Eloquent\Model;
use Overtrue\LaravelFavorite\Traits\Favoriteable;

class Post extends Model
{
    use Favoriteable;

    <...>
}
```

### API

```php
$user = User::find(1);
$post = Post::find(2);

$user->favorite($post);
$user->unfavorite($post);
$user->toggleFavorite($post);

$user->hasFavorited($post); 
$post->isFavoritedBy($user); 
```

#### Get object favoriters:

```php
foreach($post->favoriters as $user) {
    // echo $user->name;
}
```

### Aggregations

```php
// all
$user->favorites()->count(); 

// with type
$user->favorites()->withType(Post::class)->count(); 

// favoriters count
$post->favoriters()->count();
```

List with `*_count` attribute:

```php
$users = User::withCount('favorites')->get();

foreach($users as $user) {
    echo $user->favorites_count;
}
```

### N+1 issue

To avoid the N+1 issue, you can use eager loading to reduce this operation to just 2 queries. When querying, you may specify which relationships should be eager loaded using the `with` method:

```php
// Favoriter
$users = App\User::with('favorites')->get();

foreach($users as $user) {
    $user->hasFavorited($post);
}

// Favoriteable
$posts = App\Post::with('favorites')->get();
// or 
$posts = App\Post::with('favoriters')->get();

foreach($posts as $post) {
    $post->isFavoritedBy($user);
}
```


### Events

| **Event** | **Description** |
| --- | --- |
|  `Overtrue\LaravelFavorite\Events\Favorited` | Triggered when the relationship is created. |
|  `Overtrue\LaravelFavorite\Events\Unfavorited` | Triggered when the relationship is deleted. |

## Related packages

- Follow: [overtrue/laravel-follow](https://github.com/overtrue/laravel-follow)
- Like: [overtrue/laravel-like](https://github.com/overtrue/laravel-like)
- Favorite: [overtrue/laravel-favorite](https://github.com/overtrue/laravel-favorite)
- Subscribe: [overtrue/laravel-subscribe](https://github.com/overtrue/laravel-subscribe)
- Vote: overtrue/laravel-vote (working in progress)
- Bookmark: overtrue/laravel-bookmark (working in progress)


## Contributing

You can contribute in one of three ways:

1. File bug reports using the [issue tracker](https://github.com/overtrue/laravel-favorites/issues).
2. Answer questions or fix bugs on the [issue tracker](https://github.com/overtrue/laravel-favorites/issues).
3. Contribute new features or update the wiki.

_The code contribution process is not very formal. You just need to make sure that you follow the PSR-0, PSR-1, and PSR-2 coding guidelines. Any new code contributions must be accompanied by unit tests where applicable._

## PHP 扩展包开发

> 想知道如何从零开始构建 PHP 扩展包？
>
> 请关注我的实战课程，我会在此课程中分享一些扩展开发经验 —— [《PHP 扩展包实战教程 - 从入门到发布》](https://learnku.com/courses/creating-package)

## License

MIT
