# eventrebild

## Composer
> Dependency Manager for PHP

- Integreret del af Drupal 8
- drupal-composer/drupal-project er det "nye" Drush Make
- Packagist (The PHP Package Repository)
- composer.json
- composer.lock


### drupal-composer/drupal-project
https://github.com/drupal-composer/drupal-project

Dependencies:
- Core og contrib modules er i .gitignore
- Samme gælder themes og profiles (`/contrib/`)
- Custom moduler på GitHub?

Tools:
- Lokal version af Drush: `vendor/bin/drush`
- Lokal version af DrupalConsole `vendor/bin/console`


### Workflow med Composer
- `composer require drupal/devel:8.*`
- Håndtering af .lock (Merge conflicts)
- Samme version af Composer
- Langsomme kommander



## initial commit

### 1:
```
$ composer create-project drupal-composer/drupal-project:8.x-dev eventrebild --stability dev --no-interaction
```


### 2:
```
composer install
```

### 3:
Installér Drupal (med Drush eller manuelt).

### 4:
Konfigurér stien til `config`-mappen i settings.php:
```
$config_directories['sync'] = 'sites/default/config';
```

## Collaborators

### 1: Git clone
```
$ git clone https://github.com/createinside/eventrebild.git eventrebild
```

### 2: Composer install
```
$ composer install
```

### 3: Installér Drupal
Installér Drupal (med Drush eller manuelt).


### 4:
Konfigurér stien til `config`-mappen i settings.php:
```
$config_directories['sync'] = 'sites/default/config';
```

### 5:
`admin/config/development/configuration`

> The staged configuration cannot be imported, because it originates from a different site than this site. You can only synchronize configuration between cloned instances of this site.

### 6: Konfigurér Site UUID
*UUID: Universally unique identifier*


```
$ drush8 cget system.site
```

```
uuid: 6932e9b1-c28c-407d-ab00-cd4cc20828b0
name: Eventkalender
mail: christian@createinside.com
slogan: ''
page:
  403: ''
  404: ''
  front: /node
admin_compact_mode: false
weight_select_max: 100
langcode: da
default_langcode: da
```

```
$ cat sites/default/config/system.site.yml
```
```
uuid: 549c8fe7-5c29-4e04-b514-20550f92463a
name: EventRebild
mail: henbak@gmail.com
slogan: ''
page:
  403: ''
  404: ''
  front: /node
admin_compact_mode: false
weight_select_max: 100
langcode: da
default_langcode: da
```

```
$ drush8 cset system.site uuid 549c8fe7-5c29-4e04-b514-20550f92463a
```

### 6: Importér konfigurationsfiler
Importér instillinger til databasen.

```
$ drush8 cim
```

Muligvis en bug:
```
Drupal\Core\Config\ConfigImporterException: There were errors validating the config synchronization. in                   [error]
Drupal\Core\Config\ConfigImporter->validate() (line 730 of
/Users/Christian/Sites/eventrebild/web/core/lib/Drupal/Core/Config/ConfigImporter.php).
The import failed due for the following reasons:                                                                          [error]
Entities exist of type <em class="placeholder">Genvejslink</em> and <em class="placeholder"></em> <em
class="placeholder">Standard</em>. These entities need to be deleted before importing.
```

```
$ cat sites/default/config/shortcut.set.default.yml
```

```
uuid: 4784cbee-9a1c-4ccf-9735-538b170c1be2
langcode: da
status: true
dependencies: {  }
id: default
label: Standard
```

```
$ drush8 cset shortcut.set.default uuid 4784cbee-9a1c-4ccf-9735-538b170c1be2
````

```
exception 'Drupal\language\Exception\DeleteDefaultLanguageException' with message 'Can not delete the default language' in[error]
/Users/Christian/Sites/eventrebild/web/core/modules/language/src/Entity/ConfigurableLanguage.php:160
````

```
$ cat sites/default/config/language.entity.da.yml
```

```
uuid: 7c427ee9-6194-4a4f-9400-b36023191392
langcode: da
status: true
dependencies: {  }
id: da
label: Danish
direction: ltr
weight: 0
locked: false
```

```
$ drush8 cset language.entity.da uuid 7c427ee9-6194-4a4f-9400-b36023191392
```

```
Cache rebuild complete.
The configuration was imported successfully.
```
