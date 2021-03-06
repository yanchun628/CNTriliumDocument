Trilium by default comes with few color themes, with white being the default. To switch to dark theme, you just need to go to Options (top-right) -> Appearance tab and change the theme.

This is what it looks like:

[[images/dark-theme.png]]

## Custom CSS themes

Trilium provides a concept of custom user theme. You can make yourself one by creating a CSS [[code note|code notes]] and annotating it with `appTheme` [[label|attributes]].

You can see an example of what you can put there below:

```css
@font-face {
  font-family: 'Raleway';
  font-style: normal;
  font-weight: 400;
  src: url('/custom/fonts/raleway.woff2') format('woff2');
}

body.theme-my-theme {
    --main-font-family: 'Raleway' !important;
    --main-font-size: normal;
    --tree-font-family: inherit;
    --tree-font-size: normal;
    --detail-font-family: inherit;
    --detail-font-size: normal;
    --detail-text-font-family: 'Garamond' !important;

    --main-background-color: #404552;
    --main-text-color: #AFB8C6;
    --main-border-color: #AFB8C6;
    --accented-background-color: #383C4A;
    --more-accented-background-color: #2F343F;
    --header-background-color: #383C4A;
    --button-background-color: #2F343F;
    --button-disabled-background-color: #404552;
    --button-border-color: #333;
    --button-text-color: #AFB8C6;
    --button-border-radius: 2px;
    --primary-button-background-color: #6c757d;
    --primary-button-text-color: white;
    --primary-button-border-color: #6c757d;
    --muted-text-color: #86919F;
    --input-text-color: #AFB8C6;
    --input-background-color: #404552;
    --hover-item-text-color: white;
    --hover-item-background-color: #4877B1;
    --active-item-text-color: white;
    --active-item-background-color: #4877B1;
    --menu-text-color: #AFB8C6;
    --menu-background-color: #383C4A;
    --tooltip-background-color: #383C4A;
    --link-color: lightskyblue;
    --modal-background-color: #404552;
    --modal-backdrop-color: black;
    --scrollbar-border-color: rgba(175, 184, 198, 0.5);
}

body.theme-my-theme .note-detail-text {
    font-size: 120%;
}

body.theme-steel-blue .CodeMirror {
    filter: invert(100%) hue-rotate(180deg);
}
```

We define a custom font (provided by [[custom request handler]]) and then just define a bunch of CSS variables. These variables are then used in Trilium's CSS stylesheets. You can also use standard CSS selectors for further customization (open dev tools using `CTRL-SHIFT-I` to help with that), but keep in mind that HTML structure can change in future releases which might break your selectors. For that reason it is better to restrict yourself to use CSS variables as much as possible.

All selectors should be prefixed with `body.theme-my-theme` so that the stylesheet is applied only when your theme is selected. This class name is automatically created from note's name - e.g. "My theme" will transform into `theme-my-theme` (`theme-` is a common prefix). If you want to manually specify the class name you set it as a value for `appTheme` (`theme-` prefix is prepended to final class as well).

To activate your custom theme, go to Options -> Appearance. In the select box you should see all notes (themes) labeled with `appTheme`.

If you make a change to your theme, you should reload the frontend by pressing `CTRL-R` so the changes will take effect.

CSS themes can be exported in .tar archive and shared to other users. Importing CSS themes from untrusted sources is not advised since the archive can also contain executable [[scripts]] which could be potentially harmful.

You can find an example user theme *Steel Blue* in the [[demo document|Document#demo-document]]:

[[images/steel-blue.png]]

## Custom CSS

Trilium also allows you to create custom CSS not associated with a theme. This can be useful in the context of [[scripting|scripts]] where you may want to e.g. change colors of notes in the tree (as used in [[Task manager]]).

To do this, just create a [[code note|code notes]] with CSS type, put your custom CSS code into the note's content and create "appCss" [[label|Attributes]]. When Trilium frontend starts, all notes with "appCss" label are appended in the style element of the Trilium HTML page.

Once you made your changes, you can reload the Trilium frontend by pressing CTRL-R after which the changes will take effect.

[[images/app-css.png]]

## Styling the tree

If you want to give some specific notes special styling in the tree, you can give them `cssClass` [[label|attributes]] which is then put into the node representing given note in the tree.

There's also an `iconClass` using which you can define custom icons for notes in the tree - you can either use supplied ones from [boxicons](https://boxicons.com/) (e.g. `bx bx-home`) or you can define your own CSS classes.

`iconClass` and `cssClass` are especially powerful when used with [[template]].

You can also create specific styling for given note types (and mime types). For example, file note containing PNG image will have these classes in the tree: `type-image mime-image-png`.

## User provided themes

Following are themes developed by Trilium users:

* https://github.com/Abourass/TriliumThemes
* https://github.com/ZMonk91/Material-Dark-Trilium
* https://github.com/jaroet/trilium-theme-lightslategray
* https://github.com/raphwriter/trilium-theme-melon
* https://github.com/Engr-AllanG/trilium-theme-Neon_Dark