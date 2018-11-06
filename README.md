# Gutenberg Fullwidth Alignment

When we creating or adapting a wordpress theme to the new Gutenberg editor, we may encounter a problem related to the width of the element - **alignfull**. Sometimes this causes horizontal scroll bars. We can hide the horizontal scroll bars by adding **`overflow-x: hidden`** to the body tag, but sometimes the element is cut at right and left side.

There is a simple solution to this problem.

## Option 1 ##
**NOTE**: Cause various problems. Some browsers don't support CSS custom properties (variables).

1. Add JS variable to get **body** width.
```
bodyWidth = Math.max(
	document.documentElement["clientWidth"],
	document.body["scrollWidth"],
	document.documentElement["scrollWidth"],
	document.body["offsetWidth"],
	document.documentElement["offsetWidth"]
);
```
2. Add CSS variable to the **`:root`** [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)  [pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) via JS
```
document.documentElement.style.setProperty( '--fullwidth', bodyWidth + 'px' );
window.onresize = function(event) {
	document.documentElement.style.setProperty( '--fullwidth', bodyWidth + 'px' );
};
```
3. Add CSS
```
.alignfull {
	width: var(--fullwidth);
	max-width: none;
	margin: 0;
	left: 50%;
	position: relative;
	margin-left: calc( var(--fullwidth) / 2 * -1);
}
```
## Option 2 ##
Appending Style Nodes with Javascript.

1. Add JS
```
var bodyWidth = function(){
	return Math.max(
		document.documentElement["clientWidth"],
		document.body["scrollWidth"],
		document.documentElement["scrollWidth"],
		document.body["offsetWidth"],
		document.documentElement["offsetWidth"]
	);
};

function appendStyle(styles) {
	var css = document.createElement('style');
	css.type = 'text/css';
	css.id = 'alignfull-css';

	if (css.styleSheet) css.styleSheet.cssText = styles;
	else css.appendChild(document.createTextNode(styles));

	document.getElementsByTagName("head")[0].appendChild(css);
}

var styles = '.alignfull { width: '+ bodyWidth() + 'px !important }';

appendStyle(styles);

window.onresize = function(event) {
	document.getElementById('alignfull-css').remove();
	var styles = '.alignfull { width: '+ bodyWidth() + 'px !important }';
	appendStyle(styles);
};
```
2. Add CSS
```
.alignfull {
	margin: 0;
	margin-left: 50%;
	transform: translateX(-50%);
}
```

Resources:
[:root](https://developer.mozilla.org/en-US/docs/Web/CSS/:root),
[Using CSS custom properties (variables)](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)
