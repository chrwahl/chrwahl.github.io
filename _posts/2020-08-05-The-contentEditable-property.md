---
title: The contentEditable property
date: 2020-08-05 13:35:00 +02
tags: ["HTML","contenteditable"]
---

The purpose of this article is to test different HTML elements with the contenteditable attribute, only focusing on the behavior when creating a new line.

Different HTML elements behave differently and depending on the purpose of the editable content this can be an issue.
In a case where all line breaks are important, a mix of `<div>` and `<br>` elements can mess up the structure of the content. 



### Documentation:

- [https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/contentEditable](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/contentEditable)

### Testing

Try out the following <q>editors</q>. Write something in the boxes and create new line by hitting enter. My comments are based on the behavior I see in Firefox 79.

Testing `<div>`
:   <div class="testeditor">
    	<div contenteditable="true" class="editor">Write here...</div>
    	<div class="code">&lt;div contenteditable&gt;Write here...&lt;/div&gt;</div>
    </div>

    In a `<div>` a line break will brake up the content in `<div>` elements creating a `<div>` for both the content to the left and to the right of the line break.

Testing `<p>`:
:   <div class="testeditor">
    	<p contenteditable="true" class="editor">Write here...</p>
    	<div class="code">&lt;p contenteditable&gt;Write here...&lt;/p&gt;</div>
    </div>

    Line breaks will cause a `<br>` to be created.

Testing `<section>`:
:   <div class="testeditor">
    	<section contenteditable="true" class="editor">Write here...</section>
    	<div class="code">&lt;section contenteditable&gt;Write here...&lt;/section&gt;</div>
    </div>

    The `<section>` element behaves like the `<div>` element.

Testing `<span>`:
:   <div class="testeditor">
    	<span contenteditable="true" class="editor">Write here...</span>
    	<div class="code">&lt;span contenteditable&gt;Write here...&lt;/span&gt;</div>
    </div>

    The `<span>` element seems to work like the `<p>` element. The new line will be created with a `<br>`.

The last two tests already have child elements. The first one is testing a `<div>` where there is a `<p>` child element:

Testing `<div>` with `<p>`
:   <div class="testeditor">
    	<div contenteditable="true" class="editor"><p>Write here...</p></div>
    	<div class="code">&lt;div contenteditable&gt;&lt;p&gt;Write here...&lt;/p&gt;&lt;/div&gt;</div>
    </div>

    Creating a line break in the middle of the `<p>` element or in the end will end up being two `<p>` elements.

Testing `<div>` with `<h3>`
:   And the second will test a `<div>` where there is a `<h3>` child element:

    <div class="testeditor">
    	<div contenteditable="true" class="editor"><h3>Write here...</h3></div>
    	<div class="code">&lt;div contenteditable&gt;&lt;h3&gt;Write here...&lt;/h3&gt;&lt;/div&gt;</div>
    </div>

    When the cursor is in the end of a header (`<h3>`) the next line will be a `<div>` and if the cursor is in the middle of the header, it will be split in two.


### Conclusion

Different elements behave differently when they have the property `contenteditable`.
As I see it there are two options.
If the output of the <q>editor</q> is tuned into plain text (innerText) it cound be a good idea to use either the `p` or `<span>` element.
If you need a more structured output with child elements like headers, paragrafs etc. a block element like `<div>` should be used.


<style type="text/css">
	.testeditor {display: flex; justify-content: space-between;}

	.code, .editor {border: thin black solid; padding: 0 .5%; width: 48%; min-height: 50px; background-color: rgba(0,0,0,.15);}
	span.editor {display: block;}
	.code {font-family: monospace;}
</style>

<script type="text/javascript">
	document.querySelectorAll('.editor').forEach(editor => {
		editor.addEventListener('keyup', e => {
			let type = e.target.tagName.toLowerCase();
			e.target.parentElement.querySelector('div.code').innerText = `<${type} contenteditable>${e.target.innerHTML}</${type}>`;
		});
	});
</script>