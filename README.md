# PhpCodeFixer

PhpCodeFixer - a scanner that checks compatibility of your code with new interpreter versions.

[![Composer package](http://composer.network/badge/wapmorgan/php-code-fixer)](https://packagist.org/packages/wapmorgan/php-code-fixer) [![Latest Stable Version](https://poser.pugx.org/wapmorgan/php-code-fixer/v/stable)](https://packagist.org/packages/wapmorgan/php-code-fixer) [![Total Downloads](https://poser.pugx.org/wapmorgan/php-code-fixer/downloads)](https://packagist.org/packages/wapmorgan/php-code-fixer) [![License](https://poser.pugx.org/wapmorgan/php-code-fixer/license)](https://packagist.org/packages/wapmorgan/php-code-fixer)

PhpCodeFixer finds usage of deprecated functions / variables / ini-directives / constants, usage of functions with changed behavior and usage of reserved identifiers in your php code. It literally helps you fix code that can fail after migration to newer PHP version.

1. [Usage](#usage)
2. [Example](#example)
3. [Installation](#installation)

# Usage
To scan your files or folder launch `bin/phpcf` and pass file or directory names.

```
Usage: bin/phpcf [--target VERSION] [--max-size SIZE] [--exclude NAME] FILES...

Options:
  -t, --target VERSION Sets target php version [default: 7.2]
  -s, --max-size SIZE Sets max size of php file. If file is larger, it will be skipped [default: 1mb]
  -e, --exclude NAME Sets excluded file or directory names for scanning. If need to pass few names, join it with comma.
```

By providing additional parameter `--target` you can specify version of PHP to perform less checks.

| Option       | Action                                      |
|--------------|---------------------------------------------|
| --target 7.2 | By default. Use all deprecations up to 7.2. |
| --target 7.1 | Use all deprecations from 5.3 to 7.1.       |
| --target 7.0 | Use all deprecations from 5.3 to 7.0.       |
| --target 5.6 | Use all deprecations from 5.3 to 5.6.       |
| --target 5.5 | Use all deprecations from 5.3 to 5.5.       |
| --target 5.4 | Use all deprecations from 5.3 to 5.4.       |
| --target 5.3 | Use deprecations from 5.3 only.             |

# Example of usage
```
> bin/phpcf tests
Max file size set to: 1.000 MiB
Scanning tests ...
 PHP |             Type |                          File:Line | Issue
 5.3 | function         |                    tests/5.3.php:2 | Function dl() is deprecated.
 5.3 | ini              |                    tests/5.3.php:3 | Ini define_syslog_variables is deprecated.
 5.4 | function         |                    tests/5.4.php:2 | Function mcrypt_generic_end() is deprecated.
 5.4 | function         |                    tests/5.4.php:3 | Function magic_quotes_runtime() is deprecated.
 5.5 | function_usage   |                    tests/5.5.php:2 | Function usage preg_replace (@preg_replace_e_modifier) is deprecated.
 5.6 | ini              |                    tests/5.6.php:6 | Ini mbstring.http_output is deprecated.
 5.6 | variable         |                    tests/5.6.php:3 | Variable $HTTP_RAW_POST_DATA is deprecated.
 7.0 | function         |                    tests/7.0.php:8 | Function mssql_connect() is deprecated.
 7.0 | ini              |                   tests/7.0.php:10 | Ini always_populate_raw_post_data is deprecated.
 7.0 | function_usage   |                   tests/7.0.php:12 | Function usage password_hash (@password_hash_salt_option) is deprecated.
 7.0 | identifier       |                   tests/7.0.php:14 | Identifier float is reserved by PHP core.
 7.0 | method_name      |                    tests/7.0.php:3 | Method name test:test (@php4_constructors) is deprecated.
 7.1 | function         |                    tests/7.1.php:2 | Function mcrypt_decrypt() is deprecated.
 7.1 | ini              |                    tests/7.1.php:4 | Ini session.hash_function is deprecated.
 7.1 | function_usage   |                    tests/7.1.php:7 | Function usage mb_ereg_replace (@mb_ereg_replace_e_modifier) is deprecated.
 7.1 | identifier       |                    tests/7.1.php:9 | Identifier iterable is reserved by PHP core.
 7.2 | function         |                    tests/7.2.php:2 | Function create_function() is deprecated.
 7.2 | function         |                    tests/7.2.php:7 | Function read_exif_data() is deprecated.
 7.2 | constant         |                    tests/7.2.php:9 | Constant INTL_IDNA_VARIANT_2003 is deprecated.
 7.2 | ini              |                    tests/7.2.php:3 | Ini mbstring.func_overload is deprecated.
 7.2 | function_usage   |                    tests/7.2.php:5 | Function usage assert (@assert_on_string) is deprecated.
 7.2 | function_usage   |                   tests/7.2.php:12 | Function usage parse_str (@parse_str_without_argument) is deprecated.

Replace Suggestions:
1. Don't use function mcrypt_generic_end. Instead use mcrypt_generic_deinit
2. Don't use function read_exif_data. Instead use exif_read_data
3. Don't use ini mbstring.http_output. Instead use default_charset
4. Don't use variable $HTTP_RAW_POST_DATA. Instead use php://input
5. Don't use constant INTL_IDNA_VARIANT_2003. Instead use INTL_IDNA_VARIANT_UTS46
Peak memory usage: 1.062 MB
```

# Installation

## Phar
The recommended way to install _phpcf_ is as phar-package.

1. Just download a phar from [releases page](https://github.com/wapmorgan/PhpCodeFixer/releases)
2. Make it executable and put it in one of folders listed in your `$PATH`:
    ```sh
    chmod +x phpcf.phar
    sudo mv phpcf.phar /usr/local/bin/phpcf
    ```

Further I will use commands for PhpCodeFixer installed as phar or globally with composer, but if you've installed it locally with composer, just replace `phpcf` command with `vendor/bin/phpcf`.

## Composer
Another way to install _phpcf_ is via composer.

1. If you do not have composer installed, download the [`composer.phar`](https://getcomposer.org/composer.phar) executable or use the installer.

  ```sh
  $ curl -sS https://getcomposer.org/installer | php
  ```

2. Run `php composer.phar require wapmorgan/php-code-fixer` or add requirement in composer.json.

  ```json
  {
    "require": {
      "wapmorgan/php-code-fixer": "*"
    }
  }
  ```

3. Run `php composer.phar update`

### Global installation
You can get more profit when _phpcf_ installed globally.

1. If you do not have composer installed, look previous section and install composer on your server.

2. Run `php composer.phar global require wapmorgan/php-code-fixer`

If phpcf installed globally, you can use `phpcf` command inside any directory.
