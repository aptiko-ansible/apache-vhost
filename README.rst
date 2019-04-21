============
apache-vhost
============

Overview
========

This is an Ansible role for configuring apache sites on Debian. It also
configures awstats. It depends on `apache`, which must also be listed as
a role for the server. Use `apache-vhost` like this::

  - role: apache-vhost
    server_name: example.org
    server_aliases: www.example.org

Except for configuration done through the variables specified below,
other roles can also drop configuration files in
``/etc/apache2/snippets/SERVER_NAME/``, and notify the "Restart apache"
handler. These snippets are included in the configuration.

Variables
=========

- ``server_name``: The canonical server name.
- ``server_aliases``: A list of server aliases, which will be
  redirected to the canonical server name.
- ``server_aliases_without_redirect``: Server aliases to be served as
  is, not to be redirected to the canonical (this is intended for
  testing, for example when it's not yet possible to transfer the main
  domain to a new server and an alternative domain needs to be used
  while the new server is being setup).
- ``document_root``: The document root. The default is ``/var/www/{{
  server_name }}``.
- ``extras``: A string with extra configuration to be added to the
  configuration file.
- ``nonssl_extras``: Like ``extras``, but this is configuration that
  will only be used for non-SSL.
- ``ssl_extras``: Like ``extras``, but this is configuration that will
  only be used for SSL (``extras`` is also used for SSL). Ignored if
  ``cert`` is unset.
- ``letsencrypt``: Can be "true" or "false" (the default) or a string.
  If "true" or a string, the certificates are taken from directory
  ``/etc/letsencrypt``. This module does not place the certificates
  there and does not create them; it expects to find them. If it's
  "true", it uses the certificate named after the ``server_name``; if
  it's a string, it uses that string. Create the certificates separately
  by running ``certbot --apache certonly``. If ``letsencrypt`` is "true"
  or a string, ``cert``, ``private_key`` and ``chain_certificates``
  should not be specified. If neither ``letsencrypt`` nor ``cert`` is
  specified, there's no SSL.
- ``cert``: The SSL certificate (see also ``letsencrypt``).
- ``private_key``: The SSL private key.
- ``chain_certificates``:   A list of SSL chain certificates.
- ``force_ssl``: Can be "true" or "false" (the default). If "true",
  visiting the non-ssl version will redirect to the ssl version. If
  ``force_ssl`` is "true", ``cert`` or ``letsencrypt`` must be
  specified.
- ``awstats_allow_from``: IPs or subnets from which access to awstats is going
  to be allowed, in addition to the localhost. By default this is empty.

Meta
====

Written by Antonis Christofides

| Copyright (C) 2011-2015 Antonis Christofides
| Copyright (C) 2014 TEI of Epirus
| Copyright (C) 2015 National Technical University of Athens

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see http://www.gnu.org/licenses/.
