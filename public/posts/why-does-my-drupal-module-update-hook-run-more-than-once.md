That's a great question, and I am not really sure why; however, I do have an example of this and a solution that worked.

Here is a gist of an update hook that kept running each time I updated the Drupal installation:

<a href="https://gist.github.com/bighappyface/c6a8a6ea1debefaba6d7/1828c37445c46b7f4ee5e16aca0275ead796b33c">https://gist.github.com/bighappyface/c6a8a6ea1debefaba6d7/1828c37445c46b7f4ee5e16aca0275ead796b33c</a>

As described above, each time I ran <code>drush updb</code> the update hook would run.

I determined that it kept running because the module schema version was never updating to match the update number (e.g. the "N" in <em>hook_update_N</em>). I do not know why it was not updating.

My solution: I simply added the following to be run when the update was complete and the hook stopped running on subsequent site updates:

```php
drupal_set_installed_schema_version('rs', 7100);
```

<a href="https://gist.github.com/bighappyface/c6a8a6ea1debefaba6d7/ddf7dbbf44f834d8df7e673e65a3e6b5c7dbc726#file-rs_update_7100-php-L39">https://gist.github.com/bighappyface/c6a8a6ea1debefaba6d7/ddf7dbbf44f834d8df7e673e65a3e6b5c7dbc726#file-rs_update_7100-php-L39</a>

Lemme know if this makes sense or helps at all.
