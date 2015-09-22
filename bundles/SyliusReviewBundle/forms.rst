Forms
=====

The bundle ships with a default review type, that should be inherited by your application's specific review type.

Review form
-----------

The review form type can be used wherever you need, buy using form factory. Its name depends on passed ``$subject`` parameter in constructor,
which should correspond with your reviewable entity name.

.. code-block:: php

    <?php

    // src/Acme/DemoBundle/Controller/DemoController.php

    namespace Acme\DemoBundle\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Request;

    class DemoController extends Controller
    {
        public function fooAction(Request $request)
        {
            $form = $this->get('form.factory')->create('sylius_product_review');
        }
    }

The default review form consists of following fields.

+-----------------+----------+
| Field           | Type     |
+=================+==========+
| rating          | choice   |
+-----------------+----------+
| author          | text     |
+-----------------+----------+
| title           | text     |
+-----------------+----------+
| comment         | textarea |
+-----------------+----------+

.. hint::

    Form ``rating`` field, describes how many points does reviewer have to rate a reviewable entity. It's default value is 5 (1 - the worst,
    5 - the best), but it can be configured by passing chosen value with form options, under ``rating_steps`` key.

.. note::

    You may wonder why ``author`` field has type "text". This is because of ``ReviewerTransformer`` usage, that is described below.

ReviewerTransformer
-------------------

This transformer ensures proper ``ReviewType`` working. It uses passed in review form email and creates Reviewer object based on it.
