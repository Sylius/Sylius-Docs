Overriding controllers
======================

If you want to modify the controller or add your custom actions, you can do so by defining a new controller class.
By extending resource controller, you also get access to several handy methods.

.. code-block:: php

    <?php

    // src/Acme/ShopBundle/Controller/AddressController.php

    namespace Acme\ShopBundle\Controller;

    use Sylius\Bundle\ResourceBundle\Controller\ResourceController;
    use Symfony\Component\HttpFoundation\Request;

    class AddressController extends ResourceController
    {
        public function useAsBillingAction(Request $request, $id)
        {
            $user = $this->getUser();
            $address = $this->findOr404(array('id' => $id)); // Find address with given id or return 404!

            $user->setBillingAddress($address);

            // Update the user...

            return $this->redirect($this->generateUrl('acme_shop_account'));
        }
    }

Now you just need to configure your class to be used for Address controller service.

.. code-block:: yaml

    # app/config/config.yml

    sylius_addressing:
        driver: doctrine/orm
        classes:
            address:
                model: Acme\ShopBundle\Entity\Address
                controller: Acme\ShopBundle\Controller\AddressController

That's it! Now ``sylius.controller.address:recommendAction`` is available.
You can use it by defining a new route.

.. code-block:: yaml

    # app/config/routing.yml

    acme_shop_address_as_billing:
        pattern: /addresss/{id}/use-as-billing
        defaults:
            _controller: sylius.controller.address:useAsBillingAction
