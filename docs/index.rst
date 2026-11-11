Headline News Center
====================

Headline News Center is a Django application for publishing articles,
managing editorial approval, and following publishers and journalists.

Getting started
---------------

Follow the README.md in the project root to install dependencies,
configure your local settings, prepare MariaDB, and start the server.

A fresh installation has no accounts or articles. Register through the
website as a Reader, Journalist, or Editor.

Using the app
-------------

* Readers use Subscriptions to follow publishers and journalists.
  My feed displays published articles matching their subscriptions.

* Journalists use My articles to create and manage articles.
  Articles with a publisher require approval from that publisher's
  editors. Independent articles can be published by their author.

* Editors use My publishers to create publishers and manage their teams.
  Review queue contains articles submitted to their publishers.

Journalists and editors can also create newsletters containing published
articles. Subscriber emails appear in the terminal during local development.

Developer documentation
-----------------------

The reference below describes the database models, publishing services,
and access rules.

.. toctree::
   :maxdepth: 2

   reference