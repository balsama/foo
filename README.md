# Running another profile on top of lightning

This is a proof of concept for running another profile next to Lightning. See
[Make inherited install profiles load base profile modules/themes in correct order](https://www.drupal.org/node/1356276)
for more information.

Note that this is not true profile inheritence. It just allows you to run code
(modules/themes) that lives in `/profiles/b/` when `/profiles/a` is the active
profile.

## Example Setup
You can clone this repo, modify settings.php to point to your (empty) DB and run
`drush si lightning` to get a working example of an inherited profile. Or follow
the instructions below for your own project.

## Setting up your own project

__1. Create a new project from the Lightning Project__

```
$ composer create-project acquia/lightning-project:^8.1 MY_PROJECT --no-interaction --stability rc
```

__2. Require another profile in addition to lightning - in this example, Demo Framework. From within the MY_PROJECT directory:__

```
$ composer require drupal/df:dev-8.x-1.x
```

__3. Patch drupal core so that it will scan additional profile directories. In `composer.json`, add the following to the `extra` key:__

```
"patches": {
      "drupal/core": {
          "1356276 - inherited install profiles":
          "https://www.drupal.org/files/issues/make_inherited_install-1356276-133.patch"
      }
}
```

  _Note: the patch is filed against 8.2.x, but it applies cleanly to 8.1.x and works as expected in my limited testing._

__4. Add the path to the additional profile to the `profile_directories` key in $settings. In `settings.php`:__

      ````$settings['profile_directories'] = ['profiles/contrib/df'];````

_Note: Use the full path to the profile from docroot, not just the name of the directory_

__5. Install Lightning__

```
$ drush si lightning
```
 
