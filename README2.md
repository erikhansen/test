# CLS_DesignSwitcher

Uses a cookie to allow users to switch back and forth between the desktop and mobile themes as set using the native design exceptions under System -> Configuration -> Design.

## How to Use

To use this module, first install it, then add links into the footer of your mobile and desktop themes:

### Mobile Theme

Add this line of code to the footer of your mobile theme:

```html
<a href="<?php echo Mage::helper('cls_designswitcher')->getMobileToDesktopUrl() ?>">View Full Site</a>
```

The controller for the `designswitcher/switch/desktop` route will place a `USE_FULL_SITE` cookie to indicate the design exception should be ignored. There is also a route to revert to the mobile theme (`getDesktopToMobileUrl`), although at this time it will not allow users of a desktop browser to view the mobile theme by clicking the link.

### Desktop Theme

You will only want the "View Mobile Site" link to show if a user is viewing the desktop theme after having clicked on the "View Full Site" link from a mobile device. Because the link will need to be conditionally visible, you will need to include this block in your desktop theme's custom layout file (eg, local.xml):

```xml
<reference name="footer">
    <block type="cls_designswitcher/link" template="cls/designswitcher/link.phtml" name="designswitcher.link"/>
</reference>
````

*<small>The block is designed to work with Magento EE's Full Page Cache.</small>*

## Installation
1. Copy module files into Magento root either manually or by using modman

2. Open `app/etc/enterprise.xml` and change `global/cache/request_processor` to `CLS_DesignSwitcher_Model_Pagecache_Processor`

    *<small>Note: This is an important step. If missed the full page cache will not load the correct page when the cookie is set or vice-versa. There is unfortunately no other way to override the EE request processor on FPC hits.</small>*

## Compatibility
This was built and tested on EE 1.13.0.2 and then slightly modified before being published here after light testing. So it should work as described, but in case you find a bug, feel free to send a pull request.

Although it has not been installed nor tested in a Community Edition installation of Magento, it should work just fine with one caveat: the dependency on the Enterprise_PageCache module found in `app/etc/modules/CLS_DesignSwitcher.xml` must be disabled. Oh, and you can skip step 2 in the installation instructions above...
