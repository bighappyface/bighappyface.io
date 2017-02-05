# What I learned at the 2017 Drupal Global Sprint Weekend

I attended my first Drupal sprint last weekend, and I have to admit, it was much more fun and rewarding that I thought it would be. I have skipped the sprints at DrupalCon in the past, mostly to party or visit wherever the con is located, and I have regretted not making it a priority. This brief post covers my learnings and thoughts from my first Drupal sprint.

### TL;DR

1. Try to find an issue in advance. Call it pre-planning.

2. A local dev environment setup for working on Drupal core is essential. Get it going in advance, make sure it works well with a git clone of Drupal core, and seriously consider getting a solid debugger setup. (more below)

3. Know the contributor task process. Here is a great resource for contributor task guidelines: https://www.drupal.org/contributor-tasks

4. Keep participating. Practice makes perfect, and you can't get patches in core without creating and submitting them.

---

### My learning and thoughts

#### Issues for Drupal core

https://www.drupal.org/project/issues/drupal

#### Local dev env for Drupal core

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

Use xdebug when working with core

Never written a test? Learn it, look at others, and work hard to include a test in a code change contribution.

Pair up as much as possible. Even if it is just checking in with another drupaler for a fresh perspective, be sure to communicate with the in-person community before the broader community.

If you are going for the code then take it easy on the first night.
