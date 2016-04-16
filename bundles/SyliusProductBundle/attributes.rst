Product Attributes
==================

Managing Attributes
-------------------

Managing attributes happens exactly the same way like products, you have ``sylius.repository.product_attribute`` and ``sylius.manager.product_attribute`` at your disposal.

Assigning attributes to product
-------------------------------

Value of specific Attribute for one of Products is defined through AttributeValue model, which holds the references to Product, Attribute pair and the value.
If you want to programatically set an attribute value on product, use the following code.

.. code-block:: php

    <?php

    public function myAction(Request $request)
    {
        $attributeRepository = $this->container->get('sylius.repository.product_attribute');
        $attributeValueRepository = $this->container->get('sylius.repository.product_attribute_value');

        $attribute = $attributeRepository->findOneBy(array('name' => 'T-Shirt Collection'));
        $attributeValue = $attributeValueRepository->createNew();

        $attributeValue
            ->setAttribute($attribute)
            ->setValue('Summer 2013')
        ;

        $product->addAttribute($attributeValue);

        $manager = $this->container->get('sylius.manager.product');

        $manager->persist($product);
        $manager->flush(); // Save changes in database.
    }

This looks a bit tedious, doesn't it? There is a **ProductBuilder** service which simplifies the creation of products dramatically, you can learn about it in appropriate chapter.
