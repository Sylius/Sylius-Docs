Forms
=====

The bundle ships with a set of useful form types for all models. You can use the defaults or :doc:`override them </bundles/SyliusAddressingBundle/overriding_forms>` with your own forms.

Address form
------------

The address form type is named ``sylius_address`` and you can create it whenever you need, using the form factory.

.. code-block:: php

    <?php

    // src/Acme/ShopBundle/Controller/AddressController.php

    namespace Acme\ShopBundle\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Request;

    class DemoController extends Controller
    {
        public function fooAction(Request $request)
        {
            $form = $this->get('form.factory')->create('sylius_address');
        }
    }

You can also embed it into another form.

.. code-block:: php

    <?php

    // src/Acme/ShopBundle/Form/Type/OrderType.php

    namespace Acme\ShopBundle\Form\Type;

    use Sylius\Bundle\SalesBundle\Form\Type\OrderType as BaseOrderType;
    use Symfony\Component\Form\FormBuilderInterface;

    class OrderType extends BaseOrderType
    {
        public function buildForm(FormBuilderInterface $builder, array $options)
        {
            parent::buildForm($builder, $options);

            $builder
                ->add('billingAddress', 'sylius_address')
                ->add('shippingAddress', 'sylius_address')
            ;
        }
    }

The default address form consists of following fields.

+-----------------+----------+
| Field           | Type     |
+=================+==========+
| name            | text     |
+-----------------+----------+
| description     | textarea |
+-----------------+----------+
| availableOn     | datetime |
+-----------------+----------+
| metaDescription | text     |
+-----------------+----------+
| metaKeywords    | text     |
+-----------------+----------+

You can render each of these using the usual Symfony ``{{ form_row(form.description) }}``.

Property form
-------------

Default form for the Property model has name ``sylius_property`` and contains several basic fields.

+--------------+--------+
| Field        | Type   |
+==============+========+
| name         | text   |
+--------------+--------+
| presentation | text   |
+--------------+--------+
| type         | choice |
+--------------+--------+

Prototype form
--------------

Default form for the Prototype model has name ``sylius_prototype`` and is built from following fields.

+------------+------------------------+
| Field      | Type                   |
+============+========================+
| name       | text                   |
+------------+------------------------+
| properties | sylius_property_choice |
+------------+------------------------+


Miscellaneous fields
--------------------

There are few more form types, which can become useful when integrating the bundle into your app.

``sylius_address_property`` is a form which is used to set the address properties (and their values). It has 2 fields, the property choice field and a value input.

``sylius_property_choice`` is a ready to use select field, with a list of all Properties from database.

``sylius_address_to_identifier`` can be used to render a text field, which will transform the value into a address.

**If you need to customize existing fields or add your own, please read the** :doc:`overriding forms chapter </bundles/SyliusAddressingBundle/overriding_forms>`.
