Installation
============

We assume you're familiar with `Composer <http://packagist.org>`_, a dependency manager for PHP.
Use following command to add the bundle to your `composer.json` and download package.

If you have `Composer installed globally <http://getcomposer.org/doc/00-intro.md#globally>`_.

.. code-block:: bash

    $ composer require sylius/addressing-bundle:0.2.*

Otherwise you have to download .phar file.

.. code-block:: bash

    $ curl -sS https://getcomposer.org/installer | php
    $ php composer.phar require sylius/addressing-bundle:0.2.*

Adding required bundles to the kernel
-------------------------------------

You need to enable the bundle inside the kernel.

If you're not using any other Sylius bundles, you will also need to add `SyliusResourceBundle` and its dependencies to kernel.
Don't worry, everything was automatically installed via Composer.

.. code-block:: php

    <?php

    // app/AppKernel.php

    public function registerBundles()
    {
        $bundles = array(
            new FOS\RestBundle\FOSRestBundle(),
            new JMS\SerializerBundle\JMSSerializerBundle($this),
            new Stof\DoctrineExtensionsBundle\StofDoctrineExtensionsBundle(),
            new WhiteOctober\PagerfantaBundle\WhiteOctoberPagerfantaBundle(),
            new Sylius\Bundle\AddressingBundle\SyliusAddressingBundle(),
            new Sylius\Bundle\ResourceBundle\SyliusResourceBundle(),

            // Other bundles...
            new Doctrine\Bundle\DoctrineBundle\DoctrineBundle(),
        );
    }

.. note::

    Please register the bundle before *DoctrineBundle*. This is important as we use listeners which have to be processed first.

Container configuration
-----------------------

Put this configuration inside your ``app/config/config.yml``.

.. code-block:: yaml

    sylius_addressing:
        driver: doctrine/orm # Configure the doctrine orm driver used in documentation.

And configure doctrine extensions which are used in assortment bundle:

.. code-block:: yaml

    stof_doctrine_extensions:
        orm:
            default:
                timestampable: true

Routing configuration
---------------------

Add following to your ``app/config/routing.yml``.

.. code-block:: yaml

    sylius_addressing:
        resource: @SyliusAddressingBundle/Resources/config/routing.yml

Updating database schema
------------------------

Remember to update your database schema.

Run the following command.

.. code-block:: bash

    $ php app/console doctrine:schema:update --force

.. warning::

    This should be done only in **dev** environment! We recommend using Doctrine migrations, to safely update your schema.

Congratulations! The bundle is now installed and ready to use.
