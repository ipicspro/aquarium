Aquarium
========

Aquarium is a cookiecuter_ template for hassle-free
`Docker Compose`_ + Splash_ setup. Think of it as a Splash instance
with extra features and without common pitfalls.

.. _cookiecuter: http://cookiecutter.rtfd.org
.. _Splash: https://github.com/scrapinghub/splash
.. _Docker Compose: https://docs.docker.com/compose/
.. https://blog.jessfraz.com/post/tor-socks-proxy-and-privoxy-containers/

Usage
-----

First, make sure Docker and Docker Compose are installed.

Then install cookiecutter::

    pip install cookiecutter

or (on OS X + homebrew)::

    brew install cookiecutter

Then generate a folder with config files::

    cookiecutter gh:Adeptmind/aquarium
    cookiecutter gh:ipicspro/aquarium

With all default options it'll create an ``aquarium`` folder in the current
path. Go to this folder and start the Splash cluster::

    cd ./aquarium
    docker-compose up

Then use http://<host>:8050 as a regular Splash_ instance. On Linux
http://0.0.0.0:8050 should work; on OS X and Windows IP address depends on
boot2docker or docker-machine.

Check Splash Script
-------------------

This script is intended to be run as a cron job. Every time it is run it will check if each splash server is reponsive and restart ones if the response from a health check is non-200. This is needed because the official splash docker image is prone to gettign into a state where it just hangs, meaning the docker restart always option is not enough. Right now the script must be changed if the number of splash servers in the cluster changes.
Current Crontab
``*/10 * * * * bash /home/ocod/check_splash``

add to Crontab
crontab -e
add:
*/20 * * * * bash /home/py/aq/check_splash3  (3,5,7,8 splashes: fro 3 => check_splash3)

(check logs: cat /var/log/syslog)

Options
-------

When generating a config, cookiecutter will ask a bunch of questions.

* ``folder_name (default is "aquarium")`` - a name of the target folder.
* ``num_splashes (default is "3")`` - a number of Splash instances to create.
  To utilize full server capacity it makes sense to create slightly more Splash
  instances than CPU cores - e.g. on 2-core machine 3 instances often
  work best.
* ``health_check_period (default is "2")`` - time to wait between health check calls
* ``splash_version (default is "3.0")`` - a version of scrapighub/splash
  Docker image.
* ``auth_user (default is "user")``, ``auth_password (default is "userpass")``
  - HTTP Basic Auth credentials for Splash.
* ``splash_verbosity (default is "1")`` - Splash log verbosity, from 0 to 5.
* ``disable_private_mode (default is "false")`` - splash browsing setting, set to "true" to add this flag for splash
* ``max_timeout (default is "3600")`` - maximum allowed timeout.
* ``maxrss_mb (default is "3000")`` - a soft memory limit, in MB. Splash
  container will be restarted after some time if it starts to use more memory
  then this value.
* ``splash_slots (default is 5)`` - a number of Splash slots to use, i.e.
  how many render jobs to run in parallel in a single Splash process.
* ``stats_enabled (default is "1")`` - whether to enable HAProxy stats.
  If stats are enabled visit http://<host>:8036 to see stats page.
* ``stats_auth (default is "admin:adminpass")`` - HTTP Basic Auth credentials
  for HAProxy stats.
* ``tor (default is "1")`` - enter 0 to disable Tor_ support. When Tor support
  is enabled, all .onion links are opened using Tor. In addition to
  that, there is ``tor`` `Splash proxy profile`_ which you can use to render
  any page using Tor.
* ``adblock (default is "1")`` - Enter 0 to disable AdBlock Plus
   `request filters`_ (FIXME: this option is not working yet;
   filters are always available). By default, the following filters
   are available:

  * `easylist`: default set of EasyList_ filters for English;
  * `easyprivacy`: EasyPrivacy filters remove tracking scripts;
  * `easylist_noadult`: EasyList variant without filters for adult domains;
  * `fanboy-social`: removes social media content such as the Facebook like
    buttons and other widgets.
  * `fanboy-annoyance`: blocks Social Media content, in-page pop-ups
    and other annoyances; use it to decrease loading times and uncluttering
    pages. `fanboy-social` is already included in this filter.

.. _Tor: http://torproject.org
.. _Splash proxy profile: http://splash.readthedocs.org/en/latest/api.html#proxy-profiles
.. _request filters: http://splash.readthedocs.org/en/latest/api.html#request-filters
.. _EasyList: https://easylist.to/

Contributing
------------

* Source code: https://github.com/TeamHG-Memex/aquarium
* Bug tracker: https://github.com/TeamHG-Memex/aquarium/issues

License is MIT.

----

.. image:: https://hyperiongray.s3.amazonaws.com/define-hg.svg
	:target: https://www.hyperiongray.com/?pk_campaign=github&pk_kwd=aquarium
	:alt: define hyperiongray
