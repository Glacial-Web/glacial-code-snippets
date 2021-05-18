# Glacial Code Snippets

Collection of useful code snippets

### Table of Contents

| Snippets |
|------- | 
| JS |
| [Lazy loading forms on first scroll](#Lazy-loading-forms-on-first-scroll) |
| [Lazy load background images](#Lazy-load-background-images) |
| [Load page on mobile only 490px down the page](#Load-page-on-mobile-only-490px-down-the-page)|
| [Load scripts based on screen size](#Load-scripts-based-on-screen-size)|
| PHP |
| [Allow WP editor role to access 'appearance-->menu'](#Allow-WP-editor-role-to-access-appearance---menu) |
| [Add href to phone numbers in WP content](#Add-href-to-phone-numbers-in-WP-content) |
| [Remove/replace/change Yoast breadcrumbs](#Remove-replace-change-Yoast-breadcrumbs) |
| CSS |
| [Kadence Blocks eliminates tabs and converts the content into a regular column on 767px screens and lower](#Kadence-Blocks-eliminates-tabs-and-converts-the-content-into-a-regular-column-on-767px-screens-and-lower) |
| [Anchor text fix for fixed nav header](#Anchor-text-fix-for-fixed-nav-header) |

---

## JS

---

### Lazy loading forms on first scroll

Will load the specified iframe src on first detection on a scroll

```html
<!-- forms.glacial.com script begins here -->
<iframe allowTransparency="true"
        style="min-height:450px; height:inherit; overflow:auto;"
        width="100%" id="myFrame"
        name="contactform123" marginwidth="0" marginheight="0"
        frameborder="0" src="about:blank"></iframe>
<!-- forms.glacial.com script ends here -->
<script>
    window.onscroll = glacialLoadIframeOnScroll;
    var glacialScrollCounter = 0; // Global Variable
    function glacialLoadIframeOnScroll() {
        if (glacialScrollCounter == 0) {
            document.getElementById("myFrame").src = "https://forms.glacial.com/my-contact-form-FORMIDNUMBER.html";
            glacialScrollCounter++;
        }
    }
</script>
```

**[⬆ &nbsp; Back to Top](#table-of-contents)**

---

### Lazy load background images

```html

<style>
    .lazy-background {
        background-image: url(https://cdn-12c7.kxcdn.com/images/golasik_net/ph.jpg); /* Placeholder image */
    }

    .lazy-background.visible {
        background-image: url(https://cdn-12c7.kxcdn.com/images/golasik_net/Tony-Simmons_lowres.jpg);
    }
</style>

<script>
    document.addEventListener("DOMContentLoaded", function () {
        var lazyBackgrounds = [].slice.call(document.querySelectorAll(".lazy-background"));
        if ("IntersectionObserver" in window) {
            let lazyBackgroundObserver = new IntersectionObserver(function (entries, observer) {
                entries.forEach(function (entry) {
                    if (entry.isIntersecting) {
                        entry.target.classList.add("visible");
                        lazyBackgroundObserver.unobserve(entry.target);
                    }
                });
            });
            lazyBackgrounds.forEach(function (lazyBackground) {
                lazyBackgroundObserver.observe(lazyBackground);
            });
        }
    });
</script>
```

**[⬆ &nbsp; Back to Top](#table-of-contents)**

---

### Load page on mobile only 490px down the page

```js
var viewportWidth = window.innerWidth || document.documentElement.clientWidth;
if (viewportWidth < 767) {
    window.scrollTo(0, 490);
}
```

**[⬆ &nbsp; Back to Top](#table-of-contents)**

---

### Load scripts based on screen size

```js
if (window.screen.width > 991) { // Only load script if screen size is greater than 991px
    var head = document.getElementsByTagName('head')[0]; // Change to footer if it exists on the site
    var script = document.createElement('script');
    script.src = 'https://example.com/externalscript.js'; // External script url
    script.type = 'text/javascript';
    script.async = '1';
    script.setAttribute('token', '2'); // If your script has special attributes, use setAttribute, whenever you're unsure about a scriptattribute, just use this to be safe
    script.id = 'example_external';
    head.appendChild(script);
}
```

**[⬆ &nbsp; Back to Top](#table-of-contents)**

## PHP

---

### Allow WP editor role to access appearance--->menu

```php
// Allow editors to see access the Menus page under Appearance but hide other options
// Note that users who know the correct path to the hidden options can still access them
function hide_menu() {
 	$user = wp_get_current_user();
	
	// Check if the current user is an Editor
	if ( in_array( 'editor', (array) $user->roles ) ) {
		
		// They're an editor, so grant the edit_theme_options capability if they don't have it
		if ( ! current_user_can( 'edit_theme_options' ) ) {
			$role_object = get_role( 'editor' );
			$role_object->add_cap( 'edit_theme_options' );
		}
		
		// Hide the Themes page
	    remove_submenu_page( 'themes.php', 'themes.php' );
 
	    // Hide the Widgets page
	    remove_submenu_page( 'themes.php', 'widgets.php' );
	    // Hide the Customize page
	    remove_submenu_page( 'themes.php', 'customize.php' );
 
	    // Remove Customize from the Appearance submenu
	    global $submenu;
	    unset($submenu['themes.php'][6]);
	}
}
 
add_action('admin_menu', 'hide_menu', 10);
```

**[⬆ &nbsp; Back to Top](#table-of-contents)**

---

### Add href to phone numbers in WP content

```php
function glacial_add_phone_href( $content ) {
	if ( ( is_singular() ) && ( is_main_query() ) ) {
        // Simply add phone numbers here
		$phone_numbers = array(
			'(920) 452-5400',
			'(800) 551-EYES'
		);
		$replacement = [];
		foreach ( $phone_numbers as $phone_number ) {
			if ( preg_match( '/[a-z]/i', $phone_number ) ) {
				$change_letters = strtr( $phone_number, "ABCDEFGHIJKLMNOPQRSTUVWXYZ", "22233344455566677778889999" );
				$href           = preg_replace( '/\D/', '', $change_letters );
			} else {
				$href = preg_replace( '/\D/', '', $phone_number );
			}
			$html = '<a href="tel:+1' . $href . '">' . $phone_number . '</a>';
			$replacement[] = $html;
		}
		$content = str_replace( $phone_numbers, $replacement, $content );
	}
	return $content;
}
add_filter( 'the_content', 'glacial_add_phone_href' );
```

**[⬆ &nbsp; Back to Top](#table-of-contents)**

---

### Remove/replace/change Yoast breadcrumbs

```php

// Change some of the Yoast breadcrumbs
function glacial_remove_breadcrumb_link( $link_output, $link ) {
	// Uses Page Title to match item
	$text_to_remove = array('About Akari');
        
	foreach ( $text_to_remove as $item ) {
        // If match remove link and out put text only
		if ( $link['text'] == $item ) {
			$link_output = $link['text'];
		}
	}

	// Gather service pages, in this case using ACF field service_page
	$service_pages = new WP_Query (
		array(
			'post_type'  => 'page',
			'meta_key'   => 'service_page',
			'meta_value' => true,
		)
	);

	$services_text_to_add = $service_pages->posts;
    // Add Services pillar page before any service pages
	foreach ( $services_text_to_add as $text ) {
		if ( $link['text'] == $text->post_title ) {
			$link_output = '<span> <a href="' . get_permalink( 13 ) . '"> Services </a> » </span>' . $link_output;
		}
	}

	return $link_output;
}

add_filter( 'wpseo_breadcrumb_single_link', 'glacial_remove_breadcrumb_link', 10, 2 );
```
**[⬆ &nbsp; Back to Top](#table-of-contents)**

---

## CSS

---

### Kadence Blocks eliminates tabs and converts the content into a regular column on 767px screens and lower

```css
@media (max-width: 767px) {
    /** CONVERT KADENCE TAB BLOCK TO STACKED COLUMN ***/
    .kt-tabs-accordion-title {
        display: none !important;
    }

    .kt-tabs-wrap .wp-block-kadence-tab[role="tabpanel"] {
        display: block !important;
    }
}
```

**[⬆ &nbsp; Back to Top](#table-of-contents)**

---

### Anchor text fix for fixed nav header

```html

<div id="zoom" class="anchor"></div>

<style>
    .anchor {
        padding-top: 122px;
        margin-top: -122px;
    }
</style>
```

or

```html

<a id="myAnchorId" class="anchor-top-fix"></a>

<style>
    .anchor-top-fix {
        display: block;
        position: relative;
        top: -150px; /*Distance from top*/
        visibility: hidden;
    }
</style>
```

**[⬆ &nbsp; Back to Top](#table-of-contents)**

---
