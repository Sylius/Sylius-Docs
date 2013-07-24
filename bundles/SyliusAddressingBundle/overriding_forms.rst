Overriding Forms
================

There are several ways to override the default forms and you have full freedoom, you can even replace them completely.

Changing the form type class
----------------------------

To change the class behind ``sylius_address`` type or any other form from this bundle, you have to configure it in container.

Let's assume you want to add a "gender" field to the address form and remove the default "company" input.

Firstly, you have to create new ``AddressType`` class.

.. code-block:: php

    <?php

    // src/Acme/ShopBundle/Form/Type/AddressType.php

    namespace Acme\ShopBundle\Form\Type;

    use Sylius\Bundle\AddressingBundle\Form\Type\AddressType as BaseAddressType;
    use Symfony\Component\Form\FormBuilderInterface;

    class AddressType extends BaseAddressType
    {
        public function buildForm(FormBuilderInterface $builder, array $options)
        {
            parent::buildForm($builder, $options); // This will add the default fields.

            $builder
                ->remove('company')
                ->add('gender', 'choice', array(
                    'choices' => array('male', 'female')
                ))
            ;
        }
    }

Secondly, you have to configure your new class in container.

.. code-block:: yaml

    # app/config/config.yml

    sylius_addressing:
        driver: doctrine/orm
        classes:
            address:
                form: Acme\ShopBundle\Form\Type\AddressType

That's it! The new form type class will be used. This is possible for all other forms, see the :doc:`configuration reference </bundles/SyliusAddressingBundle/configuration>`.

.. note::

    Please remember, that you also need to add new fields to the model. Read the :doc:`chapter about overriding models </bundles/SyliusAddressingBundle/overriding_models>`.

Using different form type
-------------------------

Thanks to flexibility of the controllers (see :doc:`SyliusResourceBundle </bundles/SyliusResourceBundle/index>`) you can use different form type per action (per route, actually).
Below you can see usage of custom form in address create action.

.. code-block:: yaml

    # routing.yml

    app_business_address_create:
        pattern: /new-business-address
        methods: [GET, POST]
        defaults:
            _controller: sylius.controller.address:createAction
            _sylius:
                form: app_business_address

This action will use ``app_business_address`` form type instead of the default ``sylius_address``. If you wonder how to create this new form type, please check out the `Symfony documentation <http://symfony.com/doc/current/book/forms.html#creating-form-classes>`_.
