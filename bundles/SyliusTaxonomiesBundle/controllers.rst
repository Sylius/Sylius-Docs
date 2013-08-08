Using the Controllers
=====================

When using the bundle, you have access to a controller for each model.
You can use them to manipulate and manage your taxonomies.

Controller services
-------------------

All controllers in bundle are powered by :doc:`SyliusResourceBundle </bundles/SyliusResourceBundle/index>`. Please read the documentation to have a complete understanding what you can do.

Overriding controllers
----------------------

If you want to modify the controller or add your custom actions, you can do so by defining a new controller class.
By extending resource controller, you also get access to several handy methods.

.. code-block:: php

    <?php

    // src/Acme/ShopBundle/Controller/TaxonomyController.php

    namespace Acme\ShopBundle\Controller;

    use Sylius\Bundle\ResourceBundle\Controller\ResourceController;
    use Symfony\Component\HttpFoundation\Request;

    class TaxonomyController extends ResourceController
    {
        public function hideAction(Request $request, $id)
        {
            $taxonomy = $this->findOr404(array('id' => $id)); // Find taxonomy with given id or return 404!
            $taxonomy->hide(); // Add +1!

            $this->persistAndFlush($taxonomy); // Save taxonomy.

            return $this->redirect($this->generateUrl('acme_shop_homepage'));
        }
    }

Now you just need to configure your class to be used for Taxonomy controller service.

.. code-block:: yaml

    # app/config/config.yml

    sylius_taxonomies:
        driver: doctrine/orm
        classes:
            taxonomy:
                controller: Acme\ShopBundle\Controller\TaxonomyController

That's it! Now ``sylius.controller.taxonomy:hideAction`` is available.
You can use it by defining a new route.

.. code-block:: yaml

    # app/config/routing.yml

    acme_shop_taxonomy_hide:
        pattern: /taxonomys/{id}/hide
        defaults:
            _controller: sylius.controller.taxonomy:hideAction

Exactly the same works for the Taxon model. Which controller is available as ``sylius.controller.taxon``.
