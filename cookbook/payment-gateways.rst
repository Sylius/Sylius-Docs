Payment Gateways
================


Sylius by default is configured to use offline payment method, but if you want to use some payment providers (e.g. paypal or stripe) you need to configure the appropriate gateways.
Fortunately it is very easy, you need to add right configuration.

Examples
--------
.. code-block:: yaml
    payum:
        gateways:
            paypal_express_checkout:
                paypal_express_checkout_nvp:
                    username:  %paypal.express_checkout.username%
                    password:  %paypal.express_checkout.password%
                    signature: %paypal.express_checkout.signature%
                    sandbox:   %paypal.express_checkout.sandbox%
    sylius_payment:
        gateways:
            paypal_express_checkout: Paypal Express Checkout