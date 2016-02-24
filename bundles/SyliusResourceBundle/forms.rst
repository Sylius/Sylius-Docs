Forms
=====

Custom Resource Form
--------------------

Have you noticed how Sylius generates forms for you? Of course, for many use-cases you may want to create a custom form.
You need to create a simple class:

.. code-block:: php

    <?php

    // src/AppBundle/Form/Type/BookType.php

    namespace AppBundle\Form\Type;

    use Sylius\Bundle\ResourceBundle\Form\Type\AbstractResourceType;
    use Symfony\Component\Form\FormBuilderInterface;

    class BookType extends AbstractResourceType
    {
        public function buildForm(FormBuilderInterface $builder, array $options)
        {
            // Build your custom form!
            $builder->add('title', 'text');
        }

        public function getName()
        {
            return 'app_book';
        }
    }

Now, configure it under ``sylius_resource``:

.. code-block:: yaml

    sylius_resource:
        resources:
            app.book:
                classes:
                    model: AppBundle\Entity\Book
                    form:
                        default: AppBundle\Form\Type\BookType

That's it. Your new class will be used for all forms!

Using Generic Form Type
-----------------------

This bundle provides many useful generic form types, which can be use for all models in your application.

Resource to identifier form
~~~~~~~~~~~~~~~~~~~~~~~~~~~
This form type gives you the ability to transform resource to identifier and reverse this action. It extends `EntityType`_
from Symfony2. You can embed it into your custom form.

.. code-block:: php

    <?php

    // src/AppBundle/Form/Type/BookType.php

    namespace AppBundle\Form\Type;

    use Symfony\Component\Form\FormBuilderInterface;
    use Sylius\Bundle\ResourceBundle\Form\Type\AbstractResourceType;

    class BookType extends AbstractResourceType
    {
        public function buildForm(FormBuilderInterface $builder, array $options)
        {
            parent::buildForm($builder, $options);

            $builder
                ->add('group', 'sylius_group_to_identifier')
            ;
        }
    }

If you want to create this type for your custom resource you need to add configuration under ``sylius_resource``:

.. code-block:: yaml

    sylius_resource:
        resources:
            app.book:
                classes:
                    model: AppBundle\Entity\Book
                    form:
                        to_identifier: Sylius\Bundle\ResourceBundle\Form\Type\ResourceToIdentifierType

Resource from identifier form
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This form type gives you the ability to transform identifier to resource and reverse this action. It extends `EntityType`_
from Symfony2. You can embed it into your custom form.

.. code-block:: php

    <?php

    // src/AppBundle/Form/Type/BookType.php

    namespace AppBundle\Form\Type;

    use Symfony\Component\Form\FormBuilderInterface;
    use Sylius\Bundle\ResourceBundle\Form\Type\AbstractResourceType;

    class BookType extends AbstractResourceType
    {
        public function buildForm(FormBuilderInterface $builder, array $options)
        {
            parent::buildForm($builder, $options);

            $builder
                ->add('group', 'sylius_group_from_identifier')
            ;
        }
    }

If you want to create this type for your custom resource you need to add configuration under ``sylius_resource``:

.. code-block:: yaml

    sylius_resource:
        resources:
            app.book:
                classes:
                    model: AppBundle\Entity\Book
                    form:
                        from_identifier: Sylius\Bundle\ResourceBundle\Form\Type\ResourceToIdentifierType

.. _EntityType: http://symfony.com/doc/current/reference/forms/types/entity.html

Resource to hidden identifier form
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This form type gives you the ability to transform resource to identifier and reverse this action. It extends `HiddenType`_
from Symfony2. You can embed it into your custom form. It is represent as hidden input form field.

.. code-block:: php

    <?php

   // src/AppBundle/Form/Type/BookType.php

    namespace AppBundle\Form\Type;

    use Symfony\Component\Form\FormBuilderInterface;
    use Sylius\Bundle\ResourceBundle\Form\Type\AbstractResourceType;

    class BookType extends AbstractResourceType
    {
        public function buildForm(FormBuilderInterface $builder, array $options)
        {
            parent::buildForm($builder, $options);

            $builder
                ->add('group', 'sylius_group_to_hidden_identifier')
            ;
        }
    }

If you want to create this type for your custom resource you need to add configuration under ``sylius_resource``:

.. code-block:: yaml

    sylius_resource:
        resources:
            app.book:
                classes:
                    model: AppBundle\Entity\Book
                    form:
                        to_hidden_identifier: Sylius\Bundle\ResourceBundle\Form\Type\ResourceToHiddenIdentifierType

.. _HiddenType: http://symfony.com/doc/current/reference/forms/types/hidden.html

Resource Choice form
~~~~~~~~~~~~~~~~~~~~
This form type provides a ChoiceType for models from Sylius.
You can embed it into your custom form.

.. code-block:: php

    <?php

    // src/AppBundle/Form/Type/BookType.php

    namespace AppBundle\Form\Type;

    use Symfony\Component\Form\FormBuilderInterface;
    use Sylius\Bundle\ResourceBundle\Form\Type\AbstractResourceType;

    class BookType extends AbstractResourceType
    {
        public function buildForm(FormBuilderInterface $builder, array $options)
        {
            parent::buildForm($builder, $options);

            $builder
                ->add('group', 'sylius_zone_choice')
            ;
        }
    }

If you want to create choice for your custom resource you need to add configuration under ``sylius_resource``:

.. code-block:: yaml

    sylius_resource:
        resources:
            app.book:
                classes:
                    model: AppBundle\Entity\Book
                    form:
                        choice: Sylius\Bundle\ResourceBundle\Form\Type\ChoiceType
