Git
===

A good commit
-------------

The key concept is structural split of changes, meaning there is only **one
"logical change" per commit**. Good indication of non-atomic commit is a word
like *and* in the commit message.

Things to avoid
***************

- Mixing whitespace changes with functional code changes
- Mixing two unrelated functional changes
- Sending large new features in a single giant commit

Read more
*********

- `Structural split of changes`_
- `Atomic commit`_

.. _Structural split of changes:
   https://wiki.openstack.org/wiki/GitCommitMessages#Structural_split_of_changes
.. _Atomic commit: http://en.wikipedia.org/wiki/Atomic_commit



A good commit message
---------------------

Example
*******

::

    Capitalized, short (~50 chars) summary

    More detailed explanatory text, if necessary.  Wrap it to about 72
    characters or so. Refs #15.


Key concepts
************

- First line should be a **capitalized short summary** and should not end with
  a period
- First line should be **in imperative**, because it matches up with messages
  from git merge and revert:

  - DON'T DO THIS:

    ::

        added underscore.js to assets

  - Do this:

    ::

        Add underscore.js to assets

- Don't hesitate to use multiline commit messages:

  - Give further information about the commit, technical details etc.
  - Remember the blank line between summary and body!


- Reference a Github issue whenever possible:

  - Use present tense: fixes, refs

Read more
*********

- `A Note About Git Commit Messages`_
- `Closing issues via commit messages`_

.. _A Note About Git Commit Messages:
   http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
.. _Closing issues via commit messages:
   https://help.github.com/articles/closing-issues-via-commit-messages


Other best practices
--------------------

Some best practices are described Seth Robertson's article
`Commit Often, Perfect Later, Publish Once`_.

.. _Commit Often, Perfect Later, Publish Once:
   http://sethrobertson.github.io/GitBestPractices/
