# mobile-chat

This is currently a bit rough. Polish etc todo, but maybe it's a helpful resource nonetheless.

It's silly how much work we need to do to make even a mildly smooth chat UI in mobile safari. This demo strives to address assorted gnarly and/or buggy stuff:

-   Empty space showing up below focused input fields (outside of the DOM)
-   Page height not adjusting when the soft keyboard is presented
-   Keeping ONLY the chat messages scrollable (and not the rest of the page)

This is a lil SvelteKit app because that's the best way to build for the web these days. See src/routes/+page.svelte for relevant code.

Wish list (todo)

-   Page load jump (height + bottom scroll)
-   Factor out in to copy/paste-worthy component
-   Orientation handling
-   Auto-expand textarea
-   Smooth blur on scroll up (like iMessage)
-   Animate top inset on focus
