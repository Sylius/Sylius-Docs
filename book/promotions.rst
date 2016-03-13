.. index::
   single: Promotions

Promotions
==========
Promotions are a usefull part of Sylius, Promotions are a way of giving an automatic discount to the users. Promotions are devided in two major parts, the RUles and the Actions.

Promotion Rules
---------------
The rules of a promotion are a way of checking if the customer is eligeble for the promotion. Every rule is required and will be checked.
The rules are devided in two files, the service and the configuration, these can be found in Sylius/Componenet/Core/promotion/Checker and Sylius/Bundle/PromotionBundle/Form/Rule.

Promotion Actions
-----------------
The actions are the promotions, when eligeble by the Rules the action will be executed and a promotion will be added to the cart. This usualy is a negative amount.
The actions are also devided into two files, the action and the configuration, these can be fount in Sylius/Component/Core/promotion/Action and Sylius/Bundle/PromotionBundle/Form/Action.

Coupons
-------
...

Examples
--------
As an example, lets make a promotion that will give the second item of all items free if you have 2 of the same items in you cart.
First we need to make the rule:
lets call it "NrOfTheSameRule".
The first file we will make is the NrOfTheSameRuleChecker.php, this file will check if the user is eligeble for the discount.
The file looks like this:

.. code-block:: php

   <?php
   
   namespace Sylius\Component\Core\Promotion\Checker;
   
   use Sylius\Component\Core\Model\OrderInterface;
   use Sylius\Component\Promotion\Checker\RuleCheckerInterface;
   use Sylius\Component\Promotion\Model\PromotionSubjectInterface;
   
   class NrOfTheSameRuleChecker implements RuleCheckerInterface
   {
       public function isEligible(PromotionSubjectInterface $subject, array $configuration)
       {
         ...
       }
       
       public function getConfigurationFormType()
       {
           return 'sylius_promotion_rule_nr_of_the_same_configuration';
       }
   }

For purposes of the example, I have stripped the comments.
As you can see, there are two functions, the 'isEligible' will return a boolean to mark if the rule is eligible or not and 'getConfigurationFormType' to return the service name used to get the form for the settings. We will create that file in a bit.
The isEligible function has two parameters, the first, $subject, will have an instance of the subject using the promotion. through this instance you can reach the order and the user. The array $configuration stores all values entered through the form which the admin will fill in in the admin panel.

In this example the isEligible function will do this:

.. code-block:: php

   public function isEligible(PromotionSubjectInterface $subject, array $configuration)
   {
      if (!$subject instanceof OrderInterface) 
      {
         return false;
      }
      
      /* @var $item OrderItemInterface */
      foreach ($subject->getItems() as $item) 
      {
         if($item->getQuantity() == $congiration['qty'])
         {
            return true;
         }
      }
      return false;
   }

As you can see, we use the 'qty' in the array, this entry will be made by the admin in the admin panel, the name of this will be set in the configuration that we will make later.
After making this Rule we have to make the service to use it, this will be made in Sylius/Bundle/CoreBundle/Resources/config/services.xml and will look like this:

The class declaration:
   <parameter key="sylius.promotion_rule_checker.nr_of_the_same.class">Sylius\Component\Promotion\Checker\NrOfTheSameRuleChecker</parameter>


The service declaration:
   <service id="sylius.promotion_rule_checker.nr_of_the_same" class="%sylius.promotion_rule_checker.nr_of_the_same.class%">
      <tag name="sylius.promotion_rule_checker" type="nr_of_the_same" label="Number of the Same" />
   </service>

after this we have to make the configuration, first we declare the service, since we already have the file open:

The class declaration:
   <parameter key="sylius.form.type.promotion_action.nr_of_the_same_configuration.class">Sylius\Bundle\PromotionBundle\Form\Type\Action\NrOfTheSameConfigurationType</parameter>
        
The service declaration:
   <service id="sylius.form.type.promotion_action.nr_of_the_same_configuration" class="%sylius.form.type.promotion_action.nr_of_the_same_configuration.class%">
      <argument>%sylius.validation_group.promotion_action_nr_of_the_same_configuration%</argument>
      <tag name="form.type" alias="sylius_promotion_action_nr_of_the_same_configuration" />
   </service>

note that the name of this service is the same as in the 'getConfigurationFormType' funciton in the Rule.

After this we make the php class 'NrOfTheSameConfigurationType':

.. code-block:: php

   <?php

   namespace Sylius\Bundle\PromotionBundle\Form\Type\Action;
   
   use Symfony\Component\Form\AbstractType;
   use Symfony\Component\Form\FormBuilderInterface;
   use Symfony\Component\OptionsResolver\OptionsResolverInterface;
   use Symfony\Component\Validator\Constraints\NotBlank;
   use Symfony\Component\Validator\Constraints\Type;
   
   class NrOfTheSameConfigurationType extends AbstractType
   {
      protected $validationGroups;
      
      public function __construct(array $validationGroups)
      {
         $this->validationGroups = $validationGroups;
      }
      
      public function buildForm(FormBuilderInterface $builder, array $options)
      {
         ...
      }
      
      public function setDefaultOptions(OptionsResolverInterface $resolver)
      {
         $resolver
            ->setDefaults(array(
               'validation_groups' => $this->validationGroups,
            ))
         ;
      }
      
      public function getName()
      {
         return 'sylius_promotion_rule_nr_of_the_same_configuration';
      }
   }
   
the function getName and the constructor are self explaining. The interesting function is the function 'buildForm'. Here, the form used in the admin panel is build.
For this configuration we will use:

.. code-block:: php

   public function buildForm(FormBuilderInterface $builder, array $options)
   {
      $builder
         ->add('qty', 'integer', array(
            'label' => 'sylius.form.rule.item_count_configuration.qty',
            'constraints' => array(
            new NotBlank(),
            new Type(array('type' => 'numeric')),
            )
         ))
      ;
   }

Here we add a textbox to the form that will accept an integer, the name of it will be 'qty' because of this we can use it in the rule ($congiration['qty']).
If there are defaults to be set, they can be set in 'Sylius/Bundle/PromotionBundle/DependencyInjection/Configuration.php'.

The actions are basicaly the same, this time we will begin with the configuration, all the services are made in the same way, in the same file:

Sylius/Bundle/PromotionBundle/Resources/config/services.xml:
   ...
   <parameter key="sylius.form.type.promotion_action.second_free_configuration.class">Sylius\Bundle\PromotionBundle\Form\Type\ActionType</parameter>
   ...
   <service id="sylius.form.type.promotion_action.second_free_configuration" class="%sylius.form.type.promotion_action.second_free_configuration.class%">
      <argument>%sylius.validation_group.promotion_action_second_free_configuration%</argument>
      <tag name="form.type" alias="sylius_promotion_action_second_free_configuration" />
   </service>
   ...

Sylius/Bundle/CoreBundle/Resources/config/services.xml:
   ...
   <parameter key="sylius.promotion_action.second_free.class">Sylius\Component\Core\Promotion\Action\SecondFreeAction</parameter>
   ...
   <service id="sylius.promotion_action.second_free" class="%sylius.promotion_action.second_free.class%">
      <argument type="service" id="sylius.repository.adjustment" />
      <tag name="sylius.promotion_action" type="second_free" label="Second Free" />
   </service>
   ...

after the configuration, the clases will need to be made, first the configuration class:

.. code-block:: php

   <?php
   
   namespace Sylius\Bundle\PromotionBundle\Form\Type\Action;
   
   use Symfony\Component\Form\AbstractType;
   use Symfony\Component\Form\FormBuilderInterface;
   use Symfony\Component\OptionsResolver\OptionsResolverInterface;
   use Symfony\Component\Validator\Constraints\NotBlank;
   use Symfony\Component\Validator\Constraints\Type;
   
   class SecondFreeConfigurationType extends AbstractType
   {
      protected $validationGroups;
      
      public function __construct(array $validationGroups)
      {
         $this->validationGroups = $validationGroups;
      }
      
      public function buildForm(FormBuilderInterface $builder, array $options)
      {
      }
      
      public function setDefaultOptions(OptionsResolverInterface $resolver)
      {
         $resolver
            ->setDefaults(array(
            'validation_groups' => $this->validationGroups,
            ))
         ;
      }
      
      public function getName()
      {
         return 'sylius_promotion_action_fixed_discount_configuration';
      }
   }

Because, there is no need for the admin to make a choice or to provide input, we can leave the 'buildForm' function empty.
Then the action:

.. code-block:: php

   <?php
   
   namespace Sylius\Component\Core\Promotion\Action;
   
   use Sylius\Component\Core\Model\OrderInterface;
   use Sylius\Component\Core\Model\OrderItemInterface;
   use Sylius\Component\Promotion\Action\PromotionActionInterface;
   use Sylius\Component\Promotion\Model\PromotionInterface;
   use Sylius\Component\Promotion\Model\PromotionSubjectInterface;
   use Sylius\Component\Resource\Repository\RepositoryInterface;
   use Sylius\Component\Resource\Exception\UnexpectedTypeException;
   
   class SecondFreeAction implements PromotionActionInterface
   {
      protected $repository;
      
      public function __construct(RepositoryInterface $repository)
      {
         $this->repository = $repository;
      }
      
      public function execute(PromotionSubjectInterface $subject, array $configuration, PromotionInterface $promotion)
      {
         if (!$subject instanceof OrderInterface && !$subject instanceof OrderItemInterface) {
            throw new UnexpectedTypeException(
               $subject,
               'Sylius\Component\Core\Model\OrderInterface or Sylius\Component\Core\Model\OrderItemInterface'
            );
         }
         ...
      }
      
      public function revert(PromotionSubjectInterface $subject, array $configuration, PromotionInterface $promotion)
      {
         if (!$subject instanceof OrderInterface && !$subject instanceof OrderItemInterface) 
         {
            throw new UnexpectedTypeException(
               $subject,
               'Sylius\Component\Core\Model\OrderInterface or Sylius\Component\Core\Model\OrderItemInterface'
            );
         }
         
         $subject->removePromotionAdjustments();
      }
      
      public function getConfigurationFormType()
      {
         return 'sylius_promotion_action_fixed_discount_configuration';
      }
   }

in this class the function 'execute' is the most interesting function, here the discount will be applied.
The function looks like this:

.. code-block:: php

   public function execute(PromotionSubjectInterface $subject, array $configuration, PromotionInterface $promotion)
   {
      if (!$subject instanceof OrderInterface && !$subject instanceof OrderItemInterface) {
         throw new UnexpectedTypeException(
            $subject,
            'Sylius\Component\Core\Model\OrderInterface or Sylius\Component\Core\Model\OrderItemInterface'
         );
      }
      
      if (!$subject instanceof OrderInterface) 
      {
         return false;
      }
      
      /* @var $item OrderItemInterface */
      foreach ($subject->getItems() as $item) 
      {
         if($item->getQuantity() == $congiration['qty'])
         {
            $adjustment = $this->repository->createNew();
            
            $adjustment->setAmount(-$item->getUnitPrice());
            $adjustment->setLabel(OrderInterface::PROMOTION_ADJUSTMENT);
            $adjustment->setDescription($promotion->getDescription());
            
            $subject->addAdjustment($adjustment);
         }
      }
      return false;
   }

There is no need for persisting of flushing.
With this, the rule, action and all files neede for this are made, and after a cache:clear the new rule and action can be used.

Exclusive Promotions
--------------------
...

Final Thoughts
--------------
Finaly, I have one remark on it all, the services are all defined in Sylius/Bundle/CoreBundle/Resources/config/services.xml appart from the action service, I presume it is possible to also define that service in the same file, however, all other actions are defined in Sylius/Bundle/PromotionBundle/Resources/config/services.xml, therefor I do have my questions about the placement.

Learn more
----------

* ...
