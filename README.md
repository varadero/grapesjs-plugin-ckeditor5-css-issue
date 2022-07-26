# grapesjs-plugin-ckeditor5-css-issue
Repository for demonstrating CSS issue with ckeditor5 grapesjs plugin

# Clone
```shell
git clone https://github.com/varadero/grapesjs-plugin-ckeditor5-css-issue.git

cd grapesjs-plugin-ckeditor5-css-issue
```


# Install
```shell
npm install
```

# Serve
Serve the folder with any http server (like `http-server` - [https://www.npmjs.com/package/http-server](https://www.npmjs.com/package/http-server))

# The problem

GrapesJS can apply external CSS file to its canvas so the web page the user creates is styled. When we use grapesjs-plugin-ckeditor5 the user does not see the same styles applied to the content inside the ckeditor5 popup.

For example, when initializing grapesjs we can provide path to `.css` file to be applied to its canvas (by using `canvas: { styles: ['...'] }` in the configuration object):
```html
...
<div id="gjs">
    <strong>Strong text</strong>
</div>

<script type="text/javascript">
    var editor = grapesjs.init({
        container: '#gjs',
        fromElement: true,
        canvas: {
            styles: ['grapesjs-canvas-style.css']
        },
        ...
    });
...
</script>
```

If the file `grapesjs-canvas-style.css` contains the following:
```css
strong {
    color: red;
}
```

and there is text in grapesjs editor inside `<strong>....</strong>`, the text inside grapesjs editor appears red. When the user double-clicks on the text to edit it in the ckeditor5 popup, the text is not red. The reason is that grapesjs content is inside `<iframe>` and ckeditor5 plugin shows ckeditor5 popup as part of the parent document. The `.css` file is applied only to the grapesjs `<iframe>`. In order to have the same styling in the ckeditor5 popup we have to apply the same `.css` to the parent document. But this is dangerous because it can interfere with the styles of the parent document application, which is not acceptable. We also can't force the users to make the `.css` in such a way that is can be applied to both `<iframe>` and to the parent document (where the ckeditor5 plugin popup is created) without overwriting parent document styling.

What we think can overcome this problem naturally is to not use ckeditor5 popup but make it part of the grapesjs `<iframe>` so it changes the content inside the `<iframe>` - the built-in grapesjs RTE editor does this - when the user double-clicks on a text inside grapesjs canvas, the text becomes editable so the user changes the text inside the `<iframe>` and there is no need to do anything additional for the stylings to be applied.

