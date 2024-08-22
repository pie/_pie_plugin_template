# Pie Plugin Template
Just a starter template for basic WordPress plugins. Please create a new repo from this template and make the relevant edits to reflect your plugin. If you can improve this plugin template, then please feel free to raise an improvement idea on Slack for discussion.

Steps to get up and running with integrated updates:

1. Create a repository from this template and note your repository slug
1. Clone to your local machine, cd into the directory and run `./init.sh`
1. Start coding!

Deploying updates:

This plugin template is set up to work with integrated WordPress updates through the use of
[yahnis-elsts/plugin-update-checker](https://github.com/YahnisElsts/plugin-update-checker) and 
[rymndhng/release-on-push-action](https://github.com/rymndhng/release-on-push-action)

In order to deploy an update:

1. (Once) Enable Github Pages on your repository (Settings > Pages) so that a `update.json` can be read by the Update Checker in production sites.
1. Run `composer install`.
1. Add the following code to your plugin in order for the update checker to function (paths may need adjustment if this is not going in your main plugin file):
   ```/**
       * Load Composer autoloader
       */
      require_once plugin_dir_path( __FILE__ ) . 'vendor/autoload.php';

      use YahnisElsts\PluginUpdateChecker\v5\PucFactory;

      $update_checker = PucFactory::buildUpdateChecker(
          'https://pie.github.io/{your-repository-slug}/update.json',
          __FILE__,
          '{your-repository-slug}'
      );```
1. All updates can be branched out from a `next-release` branch.
1. Create a pull request to merge the `next-release` branch into `main` and add the appropriate label:
    * `release:major`
    * `release:minor`
    * `release:patch`
1. When merged, the `release.yml` workflow will update all of your version numbers and commit them back into main and create a github release with an extra artifact:
    1. `plugin-slug.zip` - the uploadable plugin for manual installation
1. Updates should then show in wp-admin for any users of the plugin

