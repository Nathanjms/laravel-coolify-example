# Update: See https://github.com/Nathanjms/laravel-docker-template for what I consider the easiest way to deploy laravel projects on Coolify, any questons please leave an issue over there!

# Laravel Coolify Example with Horizon

The magic comes from the `nixpacks.toml` file, which was based on the one provided by [@marcfowler](https://github.com/marcfowler) at https://github.com/coollabsio/coolify/discussions/2156.

Note: If you do not need horizon, then you may be able to further simplify this by using `nixpacks.no-horizon.toml` (renaming to `nixpacks.toml`) instead.

I've also added opcache, but to be honest am unsure if it's working (or needed here).

## Steps

### Step 1

Include the `nixpacks.toml` (or `nixpacks.no-horizon.toml` renamed to `nixpacks.toml`) in your root directory, then one of the following:

### Step 2

The `nixpacks` has a `[postbuild]` step to handle the cache:clear and the artisan:migrate commands, but to get supervisor to run (so we get horizon), you can either override the default `[start]`'s `cmd` with the one provided by Nixpacks and add the command to start supervisor:
```
[start]
cmd = 'node /assets/scripts/prestart.mjs /assets/nginx.template.conf /nginx.conf && (supervisord -c /etc/supervisord.conf & php-fpm -y /assets/php-fpm.conf & nginx -c /nginx.conf)'
```

However, if Nixpacks change this command in the future (unsure how likely this is), this will not update.

Instead, we can do the following to run supervisor post-build:

Add the following to the post-deployment commands section of the project on Coolify dashboard:
```
supervisord -c /etc/supervisord.conf
```

![Screenshot](https://raw.githubusercontent.com/Nathanjms/laravel-coolify-example/main/coolify-laravel.png)
