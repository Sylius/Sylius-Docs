Installation
============

We assume you're familiar with `Composer <http://packagist.org>`_, a dependency manager for PHP.
Use the following command to add the bundle to your `composer.json` and download the package.

If you have `Composer installed globally <http://getcomposer.org/doc/00-intro.md#globally>`_.

.. code-block:: bash

    $ composer require "sylius/product-bundle"

Otherwise you have to download .phar file.

.. code-block:: bash

    $ curl -sS https://getcomposer.org/installer | php
    $ php composer.phar require "sylius/locale-bundle"
    $ php composer.phar require "sylius/product-bundle"


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

            new Sylius\Bundle\ArchetypeBundle\SyliusArchetypeBundle(),
            new Sylius\Bundle\AttributeBundle\SyliusAttributeBundle(),
            new Sylius\Bundle\ProductBundle\SyliusProductBundle(),
            new Sylius\Bundle\LocaleBundle\SyliusLocaleBundle(),
            new Sylius\Bundle\ResourceBundle\SyliusResourceBundle(),
            new Sylius\Bundle\TranslationBundle\SyliusTranslationBundle(),
            new Sylius\Bundle\VariationBundle\SyliusVariationBundle(),

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

    sylius_archetype:
        classes:
            product:
                subject: Sylius\Component\Product\Model\Product
                attribute: Sylius\Component\Product\Model\Attribute
                option: Sylius\Component\Product\Model\Option
                archetype:
                    model: Sylius\Component\Product\Model\Archetype
                    repository: Sylius\Bundle\ResourceBundle\Doctrine\ORM\TranslatableEntityRepository
                    translatable:
                        targetEntity: Sylius\Component\Product\Model\ArchetypeTranslation
                archetype_translation:
                    model: Sylius\Component\Product\Model\ArchetypeTranslation

    sylius_attribute:
        driver: doctrine/orm

    sylius_product:
        driver: doctrine/orm

    sylius_locale:
        driver: doctrine/orm

    sylius_translation:
        default_mapping:
            translatable:
                field: translations
                currentLocale: currentLocale
                fallbackLocale: fallbackLocale
            translation:
                field: translatable
                locale: locale

    sylius_variation:
        driver: doctrine/orm


Then configure the locale to be used by Sylius services. If you are working with Symfony 2.6,
you can put this configuration inside your ``app/config/services.yml`` or in versions prior
to that, in ``app/config/config.yml``

.. code-block:: yaml

    parameters:
        sylius.locale: "%locale%"


Then configure Doctrine extensions which are used by the bundle.

.. code-block:: yaml

    stof_doctrine_extensions:
        orm:
            default:
                sluggable: true
                timestampable: true

Updating database schema
------------------------

Run the following command.

.. code-block:: bash

    $ php app/console doctrine:schema:update --force

.. warning::

    This should be done only in **dev** environment! We recommend using Doctrine migrations, to safely update your schema.

Congratulations! The bundle is now installed and ready to use.
