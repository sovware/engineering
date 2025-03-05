# File Structure
Plugin File Organization
------------------------

File structure unity across themes and plugins improves engineering efficiency and maintainability. We believe the following structure is segmented enough to keep projects organized—and thus maintainable—but also flexible and open ended enough to enable engineers to comfortably modify as necessary. All themes and plugins should derive from this structure:

```
│- assets/
│  │- css/ _______________________________ # See below for details
│  │- fonts/ _____________________________ # Custom/hosted fonts
│  │- images/ ____________________________ # Theme images
│  │- js/ ________________________________ # JS files
│  │- svg/ _______________________________ # Vector images that will be processed into icons
│- bin/ __________________________________ # WP-CLI and other scripts
│- build/ ________________________________ # Place to put build files
│- includes/ _____________________________ # PHP classes and files
│- languages/ ____________________________ # Translations
│- node_modules/ _________________________ # npm modules
│- src/ __________________________________ # JS source files
│- templates/ ____________________________ # Page templates
│- tests/
│  │- php/ _______________________________ # PHP testing suite
│  │- js/ ________________________________ # JavaScript testing suite
│- vendor/ _______________________________ # Composer dependencies
│- .babelrc ______________________________ # Babel config settings
│- .editorconfig _________________________ # Editor config settings
│- .eslintrc _____________________________ # ESLint config settings
│- composer.json _________________________ # Composer package file
│- gulpfile.babel.js _____________________ # Gulp config settings
│- package.json __________________________ # npm package file
│- webpack.config.babel.js _______________ # Webpack config settings
```


The `CSS` folder is described separately, below to improve readability:

```
│- assets/css/
│    │- admin/ ___________________________ # CSS for the admin
│    │- frontend/ ________________________ # CSS for the front end
│       │- base/ _________________________ # CSS at the top of the cascade
│       │- components/ ___________________ # Component-level CSS
│       │- global/ _______________________ # Variables and configs
│       │- layout/ _______________________ # Layout and helper classes
│       │- templates/ ____________________ # CSS for specific templates
│    │- shared/ __________________________ # Shared CSS between the admin and front end
```


The `src` folder is described separately, below to improve readability:

```
├── assets
├── components
├── admin
│   ├── App.vue
│   ├── main.js
│   ├── router.js
│   ├── components
│       └── ...
│   ├── views
│       └── ...
│   ├── store
│       └── ...
│   └── utils
│       └── ...
├── frontend
│   ├── App.vue
│   ├── components
│       └── ...
│   ├── main.js
│   └── router.js
├── utils
│   └── ...
├── mixins
```


Dependencies and Package Management
-----------------------------------

Projects generally use two different types of dependency management:

*   [npm](https://npmjs.org/) is used to manage relevant dependencies.
*   [Composer](https://getcomposer.org/) is used primarily for back-end (i.e. admin or PHP-based) dependencies

When and How to Use Packages
----------------------------

When choosing a third-party library for inclusion in your project, see if it’s available on npm (JavaScript) or Packagist (PHP). Additionally, WordPress plugins and themes are often available on [wpackagist.org](https://wpackagist.org/). Retrieving dependencies from a package repo helps slim down the code in our version control repos, meaning there’s less we need to retrieve when a new engineer starts on a project. It also contributes to easily keeping code up to date with security and performance improvements.

Most package managers differentiate between dependencies and devDependencies:

*   devDependencies are code, often build tools like Webpack or Gulp, needed to get a site to a production-ready state.
*   Dependencies are code actually used in the functioning of the site, like Lodash or Normalize.css.

Existing projects that weren’t built with package managers in mind offer an opportunity for engineering teams to implement them for all new development. Teams should also estimate and plan around the time needed to retrofit the existing codebase.

With some projects, using an automated dependency manager won’t make sense. In server environments like VIP, running dependency software on the server is impossible. If required repositories are private (i.e. invisible to the clients’ in-house developers), expecting the entire team to use a dependency manager is unreasonable. In these cases, the dependency, its version, and the reason for its inclusion in the project outside of a dependency manager should be documented.

If you are using a package where the naming and usage isn’t obvious to the average engineer, be sure to document its purpose in the README, style guide, or project documentation.

Selecting Packages
------------------

Packages are often a Matryoshka of their own dependencies. Though this code is almost certainly all open source, it’s not practical to apply the same scrutiny to packages’ code as is expected before selecting WordPress themes and plugins. Effective package selection, therefore, relies on other factors that engineers can quickly evaluate:

*   Is the package actively developed and supported?
*   Does the package have a solid reputation in the community?
*   How frequently have security issues been reported, and how quickly have they historically been addressed?
*   Does the package require a small number of dependencies?
*   How easily could the packages’ code be forked in case it’s abandoned or a critical issue needs to be addressed right away? When evaluating this, consider the package’s open source license along with the ease of modifying the code.

Package Versions and Lock
-------------------------

When installing a package, engineers can specify a version string the package manager uses to select an appropriate package version. Never specify an exact x.y.z version or else security, performance, and functionality upgrades won’t be available.  
Most third-party packages follow the Semantic Versioning (semver.org) system, where packages’ version numbers are defined in terms of major, minor, and patch levels. Changes to semver-compliant packages are expected to trigger a new major version when breaking backward compatibility.

Instead of a full x.y.z version, Specify major & minor versions x.y to minimize the risk of breaking changes being introduced into your project. Start version number strings with a caret (^) for most dependencies, for example ^1.2

For dependencies that don’t use semver, like many WordPress themes and plugins, engineers should still specify major and minor versions x.y. Start version numbers with a tilde (~) for most dependencies.

Modern package managers create lock files, such as npm’s “package-lock.json” and Composer’s “composer.lock”. These files record the package versions each package manager chose to satisfy the version number constraints on the current version of the platform. Lock files should be committed to project version control repos so all engineers can be on the same page.

Composer Based Theme Structure
------------------------------

We usually use [bedrock](https://github.com/roots/bedrock) for sites. Here’s how we might structure a project with Composer:

```
├── composer.json             # → Manage versions of WordPress, plugins & dependencies
├── config                    # → WordPress configuration files
│   ├── application.php       # → Primary WP config file (wp-config.php equivalent)
│   └── environments          # → Environment specific configs
│       ├── development.php   # → Development config
│       └── staging.php       # → Staging config
├── vendor                    # → Composer packages (never edit)
└── web                       # → Web root (document root on your webserver)
    ├── app                   # → wp-content equivalent
    │   ├── mu-plugins        # → Must use plugins
    │   ├── plugins           # → Plugins
    │   ├── themes            # → Themes
    │   └── uploads           # → Uploads
    ├── wp-config.php         # → Required by WP (never edit)
    ├── index.php             # → WordPress view bootstrapper
    └── wp                    # → WordPress core (never edit)
```


Here’s what `composer.json` might look like with some example plugins:

```
{
  "name": "roots/bedrock",
  "type": "project",
  "config": {
    "preferred-install": "dist"
  },
  "repositories": [
    {
      "type": "composer",
      "url": "https://wpackagist.org"
    }
  ],
  "require": {
    "php": ">=7.1",
    "composer/installers": "^1.4",
    "vlucas/phpdotenv": "^3.0.0",
    "oscarotero/env": "^1.1.0",
    "roots/wordpress": "5.2.0",
    "roots/wp-config": "1.0.0",
    "roots/wp-password-bcrypt": "1.0.0",
  },
  "require-dev": {
    "squizlabs/php_codesniffer": "^3.0.2",
    "roave/security-advisories": "dev-master"
  },
  "extra": {
    "installer-paths": {
      "web/app/mu-plugins/{$name}/": ["type:wordpress-muplugin"],
      "web/app/plugins/{$name}/": ["type:wordpress-plugin"],
      "web/app/themes/{$name}/": ["type:wordpress-theme"]
    },
    "wordpress-install-dir": "web/wp"
  },
  "scripts": {
    "post-root-package-install": [
      "php -r \"copy('.env.example', '.env');\""
    ],
    "test": [
      "phpcs"
    ]
  }
}
```


Third-Party Integrations
------------------------

Any and all third-party integrations need to be documented in an `INTEGRATIONS.md` file at the root of the project repository. This file includes a list of third-party services, which components of the project those services power, how the project interacts with the remote APIs, and when the interaction is triggered. An integration that could result in unexpected consequences during something like a migration (such as sending out a tweet) should be clearly documented.

For example:

```
## CrunchBase
Remote service for fetching funding and other investment data related to tech startups.

### Scheduling
- The CrunchBase API is used via JS in a dynamic product/company search on post edit pages
- Funding data is pulled every 6 hours by WP Cron to update cached data

### Integration Points
- /assets/js/src/crunchbase-autocomplete.js
- /includes/classes/crunchbase.php
- /includes/classes/cron.php

### Development API
- See https://somesitethatrequireslogin.com/credentials-for-project
```


### API Keys and Credentials

Authentication credentials and API keys should _never_ be hard-coded into a project. Hard-coding production credentials leads to embarrassing eventualities like posting development content to Twitter or emailing such content to clients’ mail lists.

Where possible, the project should expose a UI for entering and managing third party credentials.

If a management UI is impossible due to the nature of the project, credentials should be loaded via either PHP constants or WordPress filters. These options can – and should – default to developer credentials in the absence of production data.

```
<?php
// Production API keys should ideally be defined in wp-config.php
// This section should default to a development or noop key instead.
if ( ! defined( 'CLIENT_MANDRILL_API_KEY' ) && ! ENV_DEVELOPMENT ) {
    define( 'CLIENT_MANDRILL_API_KEY', '1234567890' );
}
```


The `ENV_DEVELOPMENT` constant should always be set to `true` for local development and should be used whenever and wherever possible to prevent production-only functionality from triggering in a local environment.

The location where other engineers can retrieve developer API keys (i.e. project management tool) can and should be logged in the `INTEGRATIONS.md` file to aid in local testing. Production API keys must _never_ be stored in the repository, neither in text files or hard-coded into the project itself.

Modular Code
------------

Every project, whether a plugin a theme or a standalone library, should be coded to be reusable and modular.

### Plugins

If the code for a project is split off into a functionality plugin, it should be done in such a way that the theme can function when the functionality plugin is disabled, broken, or missing. Each plugin should operate within its own namespace, both in terms of code isolation and in terms of internationalization.

Any functions the plugin exposes for use in a theme should be done so through actions and filters – the plugin should contain multiple calls to `add_filter()` and `add_action()` as the hooks themselves will be defined in the theme.

### Themes

Any theme dependencies on functionality plugins should be built with the use of `do_action()` or `apply_filters()`.

**In short,** changing to the default theme should not trigger errors on a site. Nor should disabling a functionality plugin – every piece of code should be decoupled and use standard WordPress paradigms (hooks) for interacting with one another.

### General Notes

Every project, whether it includes Composer-managed dependencies or not, _must_ contain a `composer.json` file defining the project so it can in turn be pulled in to _other_ projects via Composer.

### Editor Config

Every project should include an Editor Config file, `.editorconfig` in the root directory.  
This file will define and maintain consistent coding styles between the different IDEs and Code Editors used on the project.

All developers should install the corresponding Editor Config plugin for their  
preferred development editor from [EditorConfig.org](http://editorconfig.org/#download).

The editor config file with standard settings for commonly used files is shown below.

```
root = true

[*]
indent_style = space
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,css,scss}]
indent_style = space
indent_size = 4

[*.js]
indent_style = space
indent_size = 2

[{package.json,.travis.yml}]
indent_style = space
indent_size = 2
```


Developers may extend and/or customize these rules as new file formats are added to the project.

_**Credits**:_ Inspiration has been taken from [10up Engineering Best Practices](https://10up.github.io/Engineering-Best-Practices/structure/)
