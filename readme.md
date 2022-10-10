
# from a fresh box

# Install Docker
sudo apt-get remove docker docker-engine docker.io containerd runc
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install     ca-certificates     curl     gnupg     lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo systemctl start docker
sudo systemctl enable docker
sudo groupadd docker
sudo echo $USER
sudo usermod -aG docker $USER
# relog to get new docker group
groups
docker -v

# Install DDEV
curl https://apt.fury.io/drud/gpg.key | sudo apt-key add -
echo "deb https://apt.fury.io/drud/ * *" | sudo tee -a /etc/apt/sources.list.d/ddev.list
sudo apt update && sudo apt install -y ddev
ddev -v

# Install Drupal
ddev
ddev version
mkdir drupal9-test
cd drupal9-test/
ddev config --project-type=drupal9 --docroot=web --create-docroot
# .ddev/config.ports.yaml
ddev start
ddev composer create "drupal/recommended-project" --no-install
ddev composer require drush/drush --no-install
ddev composer install
ddev drush site:install
ddev drush uli
ddev launch
ddev composer require 'drupal/admin_toolbar:^3.2'
ddev exec drush en admin_toolbar


# Drupal Multisite

# https://www.drupal.org/docs/multisite-drupal/multisite-folder-structure-in-drupal

# https://github.com/drud/ddev-contrib/blob/master/recipes/drupal8-multisite/dot.ddev/config.multisite.yaml
# https://github.com/drud/ddev-contrib/blob/master/recipes/drupal8-multisite/web/sites/sites.php
# https://github.com/drud/ddev-contrib/blob/master/recipes/drupal8-multisite/web/sites/umami/settings.php
# https://github.com/drud/ddev-contrib/blob/master/recipes/drupal8-multisite/web/sites/default/settings.base.php
# https://github.com/drud/ddev-contrib/blob/master/recipes/drupal8-multisite/drush/sites/umami.site.yml
# .ddev/config.ports.yaml
