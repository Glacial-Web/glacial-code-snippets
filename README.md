# Glacial Code Snippets

Collection of useful code snippets

### Table of Contents

1 . [Lazy loading forms on first scroll](#Lazy-loading-forms-on-first-scroll)

### Lazy loading forms on first scroll

```html
<!-- forms.glacial.com script begins here -->
<iframe allowTransparency="true" style="min-height:450px; height:inherit; overflow:auto;" width="100%" id="myFrame"
        name="contactform123" marginwidth="0" marginheight="0" frameborder="0" src="about:blank"></iframe>
<!-- forms.glacial.com script ends here -->
<script>
    window.onscroll = myScroll;
    var counter = 0; // Global Variable
    function myScroll() {
        if (counter == 0) {
            document.getElementById("myFrame").src = "https://forms.glacial.com/my-contact-form-FORMIDNUMBER.html";
            counter++;
            console.log('you scrolled once');
        }
    }
</script>
```

**[⬆ Back to Top](#table-of-contents)**