# mobile-chat

This is currently a rough work in progress. Polish etc todo. Bless.

It's silly how much work we need to do to make even a mildly smooth chat UI in mobile safari. This demo strives to address assorted gnarly and/or buggy stuff:

-   Empty space showing up below focused input fields (outside of the DOM)
-   Page height not adjusting when the soft keyboard is presented
-   Keeping ONLY the chat messages scrollable (and not the rest of the page)
-   Other stuff?!

This is a lil SvelteKit app because that's the best way to build for the web these days. See src/routes/+page.svelte for relevant code.

Wish list (todo)

-   Factor out in to copy/paste-worthy component
-   Auto-expand textarea
-   Smooth blur on scroll up (like iMessage)
-   Smooth height adjustment on focus & blur (height set timing?)
