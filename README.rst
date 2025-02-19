Branding plugin for `Tutor <https://docs.tutor.overhang.io>`__
===================================================================================

This plugin, inspired in `Indigo <https://github.com/overhangio/tutor-indigo>`__,
allow changing the look and feel of Open edX installations.
It can control the appearance of both legacy pages based on HTML
(like the course catalog, dashboard, login, register, etc.), as well as
MFE components.

Installation
------------

::

    pip install git+https://github.com/aulasneo/tutor-contrib-branding

Configuration
-------------

Customize colors
~~~~~~~~~~~~~~~~

Most Bootstrap variables can be set using settings. 
These are the available variables and their defaults:

* BRANDING_PRIMARY: #0000FF
* BRANDING_SECONDARY: #454545
* BRANDING_FONT_FAMILY: <no default>
* BRANDING_BRAND: #9D0054
* BRANDING_SUCCESS: #178253
* BRANDING_INFO: #006DAA
* BRANDING_DANGER: #C32D3A
* BRANDING_WARNING: #FFD900
* BRANDING_LIGHT: #E1DDDB
* BRANDING_DARK: #273F2F
* BRANDING_ACCENT_A: #00BBF9
* BRANDING_ACCENT_B: #FFEE88
* BRANDING_BACKGROUND: #ffffff
* BRANDING_BG_PRIMARY: #ffffff
* BRANDING_BODY: #FFFFFF
* BRANDING_HOMEPAGE_BG_IMAGE: ""

You can add these settings to the ``config.yml`` file or using the
``tutor config --set "<setting>=<value>"`` command.

These settings affect the Bootstrap's ``_variables.scss`` file in the
`comprehensive theme <https://github.com/openedx/edx-platform/blob/master/lms/static/sass/partials/lms/theme/_variables.scss>`__
and in the `MFE branding module <https://github.com/openedx/brand-openedx/blob/625ad32f9cf8247522541ee77dfd574b30245226/paragon/_variables.scss>`__.

You can also add CSS overrides using the ``BRANDING_EXTRAS`` and the ``BRANDING_OVERRIDES`` variables,
to impact the `comprehensive theme <https://github.com/openedx/edx-platform/blob/master/lms/static/sass/partials/lms/theme/_extras.scss>`__
and the `MFE branding module <https://github.com/openedx/brand-openedx/blob/625ad32f9cf8247522541ee77dfd574b30245226/paragon/_overrides.scss>`__
respectively.

E.g., this setting will add a CSS block to change the color of h1 texts in all MFE:

::

    BRANDING_OVERRIDES: >-
      h1 {
            color: red;
      }

Managing fonts
~~~~~~~~~~~~~~

Set ``BRANDING_FONTS_URLS`` to a list of URLS pointing to a zipped set of font files.
Then use the ``tutor branding download-fonts`` command to download an unzip the font files
to ``$(tutor config printroot)/env/build/openedx/themes/theme/lms/static/fonts`` and
``$(tutor config printroot)/env/plugins/mfe/build/mfe/brand-openedx/fonts`` if the mfe plugin is enabled.

Tip: copy the download url from the `<https://fonts.google.com>`__ site,
for instance `<https://fonts.google.com/download?family=Roboto%20Flex>`__.

E.g., to add Roboto Flex font, set:

::

    BRANDING_FONTS_URLS:
    - https://fonts.google.com/download?family=Roboto%20Flex

Then run

::

    tutor branding download-fonts

To add a specific font definition, use the ``BRANDING_FONTS`` setting, e.g.:

::

    BRANDING_FONTS: >-
        @font-face {
            font-family: 'Roboto Flex';
            src: url('RobotoFlex-VF.woff2') format('woff2 supports variations'),
               url('RobotoFlex-VF.woff2') format('woff2-variations');
        }

Learn more about using flex fonts `here <https://web.dev/variable-fonts/>`__.

Finally, set the font family using the ``BRANDING_FONT_FAMILY`` variable:

::

    BRANDING_FONT_FAMILY: Roboto Flex


Downloading images
~~~~~~~~~~~~~~~~~~

CMS and LMS images can be included as long as they can be accessed through a HTTP(S) request.
Most important images are:

LMS:

- favicon.ico
- logo.png

CMS:

- studio-logo.png

A banner can also be added to the homepage.

E.g., to add custom logos and banner set the following:

::

    BRANDING_LMS_IMAGES:
    - filename: banner.png
      url: https://url/to/banner.png
    - filename: favicon.ico
      url: https://url/to/favicon.ico
    - filename: logo.png
      url: https://url/to/logo.png
    BRANDING_CMS_IMAGES:
    - filename: studio-logo.png
      url: https://url/to/studio-logo.png
    BRANDING_HOMEPAGE_BG_IMAGE: banner.png

Then run

::

    tutor branding download-images

Custom HTML block in home page
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can add a custom HTML code to be rendered in the home page after the banner
and before the list of courses by setting ``BRANDING_INDEX_ADDITIONAL_HTML``.

Customize HTML certificate
~~~~~~~~~~~~~~~~~~~~~~~~~~

By setting ``BRANDING_CERTIFICATE_HTML`` you can override the standard certificate with
your own HTML code.

Tip: Create a file with the HTML code (e.g., ``branding_certificate_html.html``)
and then update the configuration from the file.

::

    tutor config save --set BRANDING_CERTIFICATE_HTML="$(cat branding_certificate_html.html)"


Customizing static pages
~~~~~~~~~~~~~~~~~~~~~~~~

You can set your own HTML content to the typical static pages by setting the corresponding
variable:

- BRANDING_STATIC_TEMPLATE_404
- BRANDING_STATIC_TEMPLATE_429
- BRANDING_STATIC_TEMPLATE_ABOUT
- BRANDING_STATIC_TEMPLATE_BLOG
- BRANDING_STATIC_TEMPLATE_CONTACT
- BRANDING_STATIC_TEMPLATE_DONATE
- BRANDING_STATIC_TEMPLATE_EMBARGO
- BRANDING_STATIC_TEMPLATE_FAQ
- BRANDING_STATIC_TEMPLATE_HELP
- BRANDING_STATIC_TEMPLATE_HONOR
- BRANDING_STATIC_TEMPLATE_JOBS
- BRANDING_STATIC_TEMPLATE_MEDIA_KIT
- BRANDING_STATIC_TEMPLATE_NEWS
- BRANDING_STATIC_TEMPLATE_PRESS
- BRANDING_STATIC_TEMPLATE_PRIVACY
- BRANDING_STATIC_TEMPLATE_SERVER_DOWN
- BRANDING_STATIC_TEMPLATE_SERVER_ERROR
- BRANDING_STATIC_TEMPLATE_SERVER_OVERLOADED
- BRANDING_STATIC_TEMPLATE_SITEMAP
- BRANDING_STATIC_TEMPLATE_TOS

Usage
-----

::

    tutor plugins enable branding
    tutor branding download-images
    tutor branding download-fonts
    tutor images build openedx
    tutor images build mfe
    tutor local settheme theme

In K8s deployments, you will need to push the docker images and restart Tutor.

License
-------

This software is licensed under the terms of the AGPLv3.
