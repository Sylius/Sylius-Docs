Updating existing resource
==========================

To display an edit form of a particular resource, change it or update it via API, you should use the **updateAction** action of your **app.controller.user** service.

.. code-block:: yaml

    # routing.yml

    app_user_update:
        pattern: /users/{id}/edit
        methods: [GET, PUT]
        defaults:
            _controller: app.controller.user:updateAction

Done! Now when you go to ``/users/5/edit``, ResourceController will use the repository (``app.repository.user``) to find the user with ID === **5**.
If found it will create the ``app_user`` form, and set the existing user as data.

.. note::

    Currently, this bundle does not generate a form for you, the right form type has to be updated and registered in the container manually.

As a response, it will render the ``App:User:update.html.twig`` template with form view as the ``form`` variable and the existing User as the ``user`` variable.

Submitting the form
-------------------

You can use exactly the same route to handle the submit of the form and update the user.

.. code-block:: html

    <form method="post" action="{{ path('app_user_update', {'id': user.id}) }}">
        <input type="hidden" name="_method" value="PUT" />

On submit, the update action with method PUT, will bind the request on the form, and if it is valid it will use the right manager to persist the resource.
Then, by default it redirects to ``app_user_show`` to display the updated user, but like for creation of the resource - it's customizable.

When validation fails, it will simply render the form again.

Changing the template
---------------------

Just like for other actions, you can customize the template.

.. code-block:: yaml

    # routing.yml

    app_user_update:
        pattern: /users/{id}/edit
        methods: [GET, PUT]
        defaults:
            _controller: app.controller.user:updateAction
            _sylius:
                template: App:Backend/User:update.html.twig

Using different form
--------------------

Same way like for **createAction** you can override the default form.

.. code-block:: yaml

    # routing.yml

    app_user_update:
        pattern: /users/{id}/edit
        methods: [GET, PUT]
        defaults:
            _controller: app.controller.user:updateAction
            _sylius:
                template: App:Backend/User:update.html.twig
                form: app_user_custom

Overriding the criteria
-----------------------

By default, the **updateAction** will look for the resource by id. You can easily change that criteria.

.. code-block:: yaml

    # routing.yml

    app_user_update:
        pattern: /users/{username}/edit
        methods: [GET, PUT]
        defaults:
            _controller: app.controller.user:updateAction
            _sylius:
                criteria: { username: $username }

Custom redirect after success
-----------------------------

By default the controller will try to get the id of resource and redirect to the "show" route. To change that, use the following configuration.

.. code-block:: yaml

    # routing.yml

    app_user_update:
        pattern: /users/{id}/edit
        methods: [GET, PUT]
        defaults:
            _controller: app.controller.user:updateAction
            _sylius:
                redirect: app_user_index

You can also perform more complex redirects, with parameters. For example...

.. code-block:: yaml

    # routing.yml

    app_user_update:
        pattern: /competition/{competitionId}/users/{id}/edit
        methods: [GET, PUT]
        defaults:
            _controller: app.controller.user:updateAction
            _sylius:
                redirect:
                    route: app_competition_show
                    parameters: { id: $competitionId }
