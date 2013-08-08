Overriding Models
=================

The default models are already usable, but you can customize or replace them.

Using your own Address class
----------------------------

If you want to add new fields to your Address class, you have to create your own **Address** model, living inside your application code.

We think that **keeping the app-specific bundle structure simple** is a good practice, so
let's assume you have your ``ShopBundle`` registered under ``Acme\ShopBundle`` namespace.

In this example, we'll add a "phone" field.

.. code-block:: php

    <?php

    // src/Acme/ShopBundle/Entity/Address.php
    namespace Acme\ShopBundle\Entity;

    use Sylius\Bundle\AddressingBundle\Model\Address as BaseAddress;

    class Address extends BaseAddress
    {
        private $phone;

        public function getPhone()
        {
            return $this->phone;
        }

        public function setPhone($phone)
        {
            $this->phone = $phone;
        }
    }

Now define the entity mapping inside ``Resources/config/doctrine/Address.orm.xml`` of your bundle and add the new field.

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>

    <doctrine-mapping xmlns="http://doctrine-project.org/schemas/orm/doctrine-mapping"
                      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                      xsi:schemaLocation="http://doctrine-project.org/schemas/orm/doctrine-mapping
                                          http://doctrine-project.org/schemas/orm/doctrine-mapping.xsd">

        <entity name="Acme\ShopBundle\Entity\Address" table="sylius_address">
            <field name="phone" column="phone" type="string" nullable="true" />
        </entity>

    </doctrine-mapping>

Configure the new model.

.. code-block:: yaml

    # app/config/config.yml

    sylius_address:
        driver: doctrine/orm
        classes:
            address:
                model: Acme\ShopBundle\Entity\Address

Update the database schema.

.. code-block:: bash

    $ php app/console doctrine:schema:update --force

.. warning::

    This should be done only in **dev** environment! We recommend using Doctrine migrations, to safely update your schema.

Done, now the new Address model is used.
