# This file is intended to be used in the farmOS container.
# To run it locally, setup LocalRepoPath and uncomment the following lines:
# DRUPAL_PATH:=<LocalRepoPath>/docker/www
DRUPAL_PATH:=/opt/drupal
MAKEFLAGS += -j2
export

phpcs:
	phpcs ${DRUPAL_PATH}/web/profiles/farm

phpstan:
	phpstan analyze --memory-limit 1G ${DRUPAL_PATH}/web/profiles/farm

compatibility-modules:
	phpcs --standard=PHPCompatibility --runtime-set testVersion 8.2- --warning-severity=0 ${DRUPAL_PATH}/web/modules
compatibility-themes:
	phpcs --standard=PHPCompatibility --runtime-set testVersion 8.2- --warning-severity=0 ${DRUPAL_PATH}/web/themes

php-compatibility: compatibility-modules compatibility-themes

test-farm:
	# This particular test requires the most time for completion.
	phpunit --group farm ${DRUPAL_PATH}/web/profiles/farm

test-all-but-farm:
	phpunit --exclude-group farm ${DRUPAL_PATH}/web/profiles/farm

phpunit-tests: test-farm test-all-but-farm

drush: # u=<db-url>
	drush site-install --db-url=$(u) farm farm.modules='all'
