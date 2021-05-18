# Glacial Code Snippets

Collection of useful code snippets

### Table of Contents

1 . [Lazy loading forms on first scroll](#Lazy-loading-forms-on-first-scroll)

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