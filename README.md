# Glacial Code Snippets

Collection of useful code snippets

### Table of Contents
| Number | Snippets |
|------- | --------- |
|1. | [Lazy loading forms on first scroll](#Lazy-loading-forms-on-first-scroll) |
|2. | [Allow Editor Role to access 'appearance-->menu'](#Allow-Editor-Role-to-access-appearance---menu) |
|3. |
|4. |
|5. |
|6. |
|7. |
|8. |
|9. |
|10. |
|11. |
|12. |
|13. |

### Allow Editor Role to access appearance--->menu
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
**[⬆ Back to Top](#table-of-contents)**


### Lazy loading forms on first scroll
Will load the specified iframe on first detection on a scroll
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

**[⬆ Back to Top](#table-of-contents)**