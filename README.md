# ETL Adapter: JSON

# Contributing

This repo is **READ ONLY**, in order to contribute to Flow PHP project, please
open PR against [flow](https://github.com/flow-php/flow) monorepo.

Changes merged to monorepo are automatically propagated into sub repositories.

## Description

ETL Adapter that provides memory safe JSON support for ETL.

Following implementation are available: 
- [JSON Machine](https://github.com/halaxa/json-machine) 

## Installation

```
composer require flow-php/etl-adapter-json:1.x@dev
```


> Json library is not explicitly required, you need to make sure it is available in your composer.json file.
> If you are only using Loader, this dependency is optional.

## Extractor - JSONMachine - JsonExtractor

```php
<?php

use Flow\ETL\Stream\LocalFile;
use Flow\ETL\Adapter\JSON\JSONMachine\JsonExtractor;

$rows = (new Flow())
    ->read(Json::from(new LocalFile(__DIR__ . '/../Fixtures/timezones.json'), 5))
    ->fetch()
```

## Loader - JsonLoader

```php
<?php

use Flow\ETL\Adapter\JSON\JsonLoader;
use Flow\ETL\Flow;
use Flow\ETL\Row;
use Flow\ETL\Rows;
use Flow\ETL\Stream\LocalFile;

(new Flow())
    ->process(
        new Rows(
            ...\array_map(
                fn (int $i) : Row => Row::create(
                    new Row\Entry\IntegerEntry('id', $i),
                    new Row\Entry\StringEntry('name', 'name_' . $i)
                ),
                \range(0, 10)
            )
        )
    )
    ->write(Json::to(new LocalFile(\sys_get_temp_dir() . '/file.json')))
    ->run();
```

## Development

In order to install dependencies please, launch following commands:

```bash
composer install
```

## Run Tests

In order to execute full test suite, please launch following command:

```bash
composer build
```

It's recommended to use [pcov](https://pecl.php.net/package/pcov) for code coverage however you can also use
xdebug by setting `XDEBUG_MODE=coverage` env variable.
