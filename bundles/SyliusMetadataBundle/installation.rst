Installation
============

Requirements
------------

`MetadataBundle` minimal requirements are PHP 5.5.9 and Symfony 2.7 - make sure you have them.

Installation
------------

We assume you're familiar with `Composer <http://packagist.org>`_, a dependency manager for PHP.
Use the following command to add the bundle to your `composer.json` and download the package.

If you have `Composer installed globally <http://getcomposer.org/doc/00-intro.md#globally>`_.

.. code-block:: bash

    $ composer require sylius/metadata-bundle

Otherwise you have to download .phar file first.

.. code-block:: bash

    $ curl -sS https://getcomposer.org/installer | php
    $ php composer.phar require sylius/metadata-bundle

Adding required bundles to the kernel
-------------------------------------

You need to enable the bundle inside the kernel, usually at the end of bundle list.

.. code-block:: php

    <?php

    // app/AppKernel.php

    public function registerBundles()
    {
        $bundles = array(
            // Other bundles...
            new Sylius\Bundle\MetadataBundle\SyliusMetadataBundle(),
        );
    }