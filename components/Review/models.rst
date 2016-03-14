Models
======

Review
------

``Review`` is the main model in SyliusReviewComponent. This simple class provides basic properties, which every review should be described with.
The default model contains the following attributes with appropriate setters and getters.

+---------------+-------------------------------------------------------------+
| Attribute     | Description                                                 |
+===============+=============================================================+
| id            | Unique id of the review                                     |
+---------------+-------------------------------------------------------------+
| title         | Title of the review                                         |
+---------------+-------------------------------------------------------------+
| rating        | Review rating, which simply describes review author opinion |
+---------------+-------------------------------------------------------------+
| comment       | Some additional words from review author                    |
+---------------+-------------------------------------------------------------+
| author        | Instance of object describing review author                 |
+---------------+-------------------------------------------------------------+
| status        | Status of the review                                        |
+---------------+-------------------------------------------------------------+
| reviewSubject | Object, that review is describing                           |
+---------------+-------------------------------------------------------------+
| createdAt     | Review creation date                                        |
+---------------+-------------------------------------------------------------+
| updatedAt     | Review update date                                          |
+---------------+-------------------------------------------------------------+

.. note::

    This model implements ``ReviewInterface``

Reviewer
--------

``Reviewer`` is default review author class in SyliusReviewComponent. Provides all required fields with appropriate getters and setter, that are crucial for review author identification.

+---------------+-------------------------------------------------------------+
| Attribute     | Description                                                 |
+===============+=============================================================+
| id            | Unique id of the reviewer                                   |
+---------------+-------------------------------------------------------------+
| email         | Reviewer email                                              |
+---------------+-------------------------------------------------------------+
| firstName     | Reviewer first name                                         |
+---------------+-------------------------------------------------------------+
| lastName      | Reviewer last name                                          |
+---------------+-------------------------------------------------------------+

.. note::

    This model implements ``ReviewerInterface``

.. attention::

    See `ReviewerInterface` section to find out how Reviewer can be configured in your application.

ReviewableInterface
-------------------

To characterize object, that can be reviewed, its class needs to implement the ``ReviewableInterface``.

+---------------------------------------+-----------------------------------------------------------+
| Method                                | Description                                               |
+=======================================+===========================================================+
| getName()                             | Name of reviewable object                                 |
+---------------------------------------+-----------------------------------------------------------+
| getReviews()                          | Returns collection of reviews associated with this object |
+---------------------------------------+-----------------------------------------------------------+
| addReview(ReviewInterface $review)    | Adds review to object's reviews collection                |
+---------------------------------------+-----------------------------------------------------------+
| removeReview(ReviewInterface $review) | Removes review from object's reviews collection           |
+---------------------------------------+-----------------------------------------------------------+
| setAverageRating($averageRating)      | Sets average rating of object                             |
+---------------------------------------+-----------------------------------------------------------+
| getAverageRating()                    | Returns average rating of object                          |
+---------------------------------------+-----------------------------------------------------------+


ReviewerInterface
-----------------

To characterize review author object, its class needs to implement the ``ReviewerInterface``. This interface is used
as a marker, on ``Reviewer`` class and all classes that will be used as reviewer, but it also contains some crucial methods, that every review author must implement.