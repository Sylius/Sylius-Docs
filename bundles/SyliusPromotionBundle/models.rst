Models
======

All the models of this bundle are defined in ``Sylius\Bundle\PromotionBundle\Model``.

Rule
----
A ``Rule`` is used to check if your order is eligible to the promotion. A promotion can have none, one or several rules. ``SyliusPromotionBundle`` comes with 2 types of rules :

 - item count rule : the number of items of the order is checked
 - item total rule : the amount of the order is checked

A rule is configured via the ``configuration`` attribute which is an array serialized into database. For item count rules, you have to configure the ``count`` key, whereas the ``amount`` key is used for item total rules. For both types of rules, you can configure the ``equal`` key to ``false`` or ``true`` depending is you want the rule to be strict or not.
For instance, for an item count rule configured with ``equal`` to ``false`` and ``count`` to 4, orders with only **more** than 4 items will be eligible.

Action
------

An ``Action`` defines the the nature of the discount. Common actions are :

- percentage discount
- fixed amount discount

An action is configured via the ``configuration`` attribute which is an array serialized into database. For percentage discount actions, you have to configure the ``percentage`` key, whereas the ``amount`` key is used for fixed discount rules.

Coupon
------

A ``Coupon`` is a ticket having a ``code`` that can be exchanged for a financial discount. A promotion can have none, one or several coupons.

A coupon is considered as valid if the method ``isValid()`` returns ``true``. This method checks the number of times this coupon can be used (attribute ``usageLimit``), the number of times this has already been used (attribute ``used``) and the coupon expiration date (attribute ``expiresAt``). If ``usageLimit`` is not set, the coupon will be usable an unlimited times.

PromotionSubjectInterface
-------------------------

A ``PromotionSubjectInterface`` is the object you want to apply the promotion on. For instance, in Sylius Standard, a ``Sylius\Bundle\CoreBundle\Model\Order`` can be subject to promotions.

By implementing ``PromotionSubjectInterface``, your object will have to define the following methods:



======================================= ============ ====
Method signature                        return type  
======================================= ============ ====
``getPromotionSubjectTotal``            int          should return the total price for the subject (i.e. Order)
``getPromotions()``                     Collection   should return attached promotion set
``hasPromotion(PromotionInterface)``    bool         check promotion set for given promotion
``addPromotion(PromotionInterface)``    ``$this``    add to promotion set
``removePromotion(PromotionInterface)`` ``$this``    remove from promotion set
==================================================== ====

Promotion
---------

The ``Promotion`` is the main model of this bundle. A promotion has a ``name``, a ``description`` and :

- can have none, one or several rules
- should have at least one action to be effective
- can be based on coupons
- can have a limited number of usages by using the attributes ``usageLimit`` and ``used``. When ``used`` reaches ``usageLimit`` the promotion is no longer valid. If ``usageLimit`` is not set, the promotion will be usable an unlimited times.
- can be limited by time by using the attributes ``startsAt`` and ``endsAt``

