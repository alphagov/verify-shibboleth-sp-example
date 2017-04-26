verify-shibboleth-sp-example
============================

An example configuration for a Shibboleth SP instance that works with GOV.UK Verify.

How to use this configuration
-----------------------------

There are example configuration files for iis and apache. We recommend you install
the Shibboleth SP and get it running on your system before referring to these files.

Once you have a running version of the SP you can look through the files in this
project and configure your instance similarly.

Note that this example configuration is intended to be a starting point towards building a
secure, production ready setup.  If you're running locally and don't have HTTPS set up
then you will need to change some of the settings (e.g. `allowedSchemes`).

Things you'll need to replace
-----------------------------

### Generally

* `entityID="SERVICE-PROVIDER-ENTITY-ID"` - should be your service's entityId.
* `<Errors redirectErrors="SERVICE-PROVIDER-ERROR-PAGE" />` - if you have an error page to
  redirect users to, this should be its url. Shibboleth allows for other ways of handling
  errors if this doesn't suit - see the docs.
* `uri="http://MSA-HOSTNAME:MSA-PORT/matching-service/SAML2/metadata"` - should be the URL
  of your matching service adapter.
* `<CredentialResolver ...>` - There are two of these, which tell shibboleth where your
  signing and encryption keys and certificates live. If you're interacting with compliance
  tool these can be self-signed, otherwise they should be issued by Verify.

### For IIS

* `Site`: attribute `id="IIS-ID-OF-THE-WEB-SITE"` - this should be the site id in IIS,
  which you can see by looking at "Advanced Settings" for a website in the IIS interface.
* `Site`: attribute `name="SERVICE-PROVIDER-CANONICAL-HOST-NAME"` - should be the host of
  your service (localhost if running locally)
* `<Path name="SOME-SECURED-PATH"` - you should have a path element covering every path you
  want Shibboleth to protect (e.g. /secure). See [the Shibboleth docs](https://wiki.shibboleth.net/confluence/display/SHIB2/NativeSPRequestMapPath).

### For Apache

* `spoofKey="SOME-RANDOM-STRING">` - this should be set to some reasonably long random
  value. See [the Shibboleth
  docs](https://wiki.shibboleth.net/confluence/display/SHIB2/NativeSPInProcess).
* `LoadModule mod_shib /usr/local/opt/shibboleth-sp/lib/shibboleth/mod_shib_24.so` - this
  path needs to point your `mod_shib_24.so` (by default this will be in the shibboleth
  installation)
* `<Location /secure>` - Configure the paths you want Shibboleth to require a valid
  session for.

Design Decisions
----------------

We promote https only instead of http. By default the Shibboleth messages
containing user attributes are encrypted. Therefore, they can also be sent
via the insecure HTTP protocol. However, successfully authenticated Shibboleth
users accessing a web page via HTTP are prone to session hijacking attacks.

We provide only configurations that override the defaults in order to keep it simple.

This configuration is for shibboleth 2.6

Values that need attention are writen IN-CAPITALS.

