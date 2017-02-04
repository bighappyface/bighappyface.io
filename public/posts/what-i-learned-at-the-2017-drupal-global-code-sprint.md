# What I learned at the 2017 Drupal Global Sprint

## Resources

https://www.drupal.org/contributor-tasks
https://www.drupal.org/node/2610510

## My issue

https://www.drupal.org/node/2575535#comment-11898482

## Things I learned at the Drupal Sprint

### TL;DR

1. Try to find an issue in advance

2. Local dev environment for core is essential (git repo in particular). Get it going in advance.

```
# Clone repo
git clone git://git.drupal.org/project/drupal.git
# Navigate 
cd drupal
# Install composer deps
composer install
# Initialize settings.php and sqlite db
drush qd --use-existing
```

3. Know the process. Here is a great resource for contributor task guidelines: https://www.drupal.org/contributor-tasks

4. Keep participating. Practice makes perfect.

### More

Use xdebug when working with core

Never written a test? Learn it, look at others, and work hard to include a test in a code change contribution.

Pair up as much as possible. Even if it is just checking in with another drupaler for a fresh perspective, be sure to communicate with the in-person community before the broader community.

If you are going for the code then take it easy on the first night.
