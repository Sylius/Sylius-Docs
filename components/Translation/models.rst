Models
======

.. _component_translation_model_translatable-trait:

TranslatableTrait
-----------------

This trait should be used by any model which needs
different presentations of its fields in various locales.

+--------------------+-------------------------------------------------------------------------+
| Property           | Description                                                             |
+====================+=========================================================================+
| translations       | List of objects implementing the **AbstractTranslation** class          |
+--------------------+-------------------------------------------------------------------------+
| currentLocale      | Currently set locale                                                    |
+--------------------+-------------------------------------------------------------------------+
| currentTranslation | Translation chosen from translations list accordingly to current locale |
+--------------------+-------------------------------------------------------------------------+
| fallbackLocale     | Locale used in case no translation is available for the current one     |
+--------------------+-------------------------------------------------------------------------+

.. note::
   Models using it should implement the :ref:`component_translation_model_translatable-interface`. |br|
   For more detailed information go to `Sylius API TranslatableTrait`_.

.. _Sylius API TranslatableTrait: http://api.sylius.org/Sylius/Component/Translation/Model/TranslatableTrait.html

.. _component_translation_model_abstract-translation:

AbstractTranslation
-------------------

This class should be extended by a model responsible of
maintaining a single translation for provided locale.

+--------------+---------------------------------------------------------+
| Property     | Description                                             |
+==============+=========================================================+
| locale       | Translation's locale                                    |
+--------------+---------------------------------------------------------+
| translatable | The translatable model associated with this translation |
+--------------+---------------------------------------------------------+

.. note::
   This model implements the :ref:`component_translation_model_translation-interface`. |br|
   For more detailed information go to `Sylius API AbstractTranslation`_.

.. _Sylius API AbstractTranslation: http://api.sylius.org/Sylius/Component/Translation/Model/AbstractTranslation.html
