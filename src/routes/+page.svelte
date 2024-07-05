<script lang="ts">
    import { onMount } from 'svelte';

    let userMessage: string = '';
    let messageInput: HTMLTextAreaElement;
    let chatContainer: HTMLElement;
    let messagesContainer: HTMLElement;
    let inputContainer: HTMLElement;
    let topGap: HTMLElement;

    onMount(() => {
        const updateVh = () => {
            let vh = window.innerHeight * 0.01;
            document.documentElement.style.setProperty('--vh', `${vh}px`);
        };
        updateVh();
        window.addEventListener('resize', updateVh);
        window.addEventListener('orientationchange', updateVh);

        // Prevent scrolling the page (into empty space) from touches on the input container
        const preventScroll = (event: TouchEvent) => {
            event.preventDefault();
        };
        if (inputContainer) {
            inputContainer.addEventListener('touchmove', preventScroll, { passive: false });
        }

        // Adjust the viewport height when the window is resized (including soft keyboard in/out)
        if (window.visualViewport) {
            window.visualViewport.addEventListener('resize', adjustViewportHeight);
            window.visualViewport.addEventListener('scroll', adjustViewportHeight);
            window.removeEventListener('resize', updateVh);
            window.removeEventListener('orientationchange', updateVh);
        } else {
            window.addEventListener('resize', adjustViewportHeight);
            window.addEventListener('orientationchange', adjustViewportHeight);
        }

        // Set viewport height on load
        adjustViewportHeight();
        messagesContainer.scrollTop = messagesContainer.scrollHeight;

        // Clean everything up
        return () => {
            if (inputContainer) {
                inputContainer.removeEventListener('touchmove', preventScroll);
            }

            if (window.visualViewport) {
                window.visualViewport.removeEventListener('resize', adjustViewportHeight);
                window.visualViewport.removeEventListener('scroll', adjustViewportHeight);
            } else {
                window.removeEventListener('resize', adjustViewportHeight);
                window.removeEventListener('orientationchange', adjustViewportHeight);
            }
        };
    });

    function adjustViewportHeight() {
        if (window.visualViewport) {
            // Adding the margin top makes the full height of the content visible, since mobile safari
            // doesn't resize the page when the keyboard presents (instead just pushes it up)
            const viewportHeight = window.visualViewport.height;
            const contentHeight = chatContainer.scrollHeight;
            const heightDiff = contentHeight - viewportHeight;
            messagesContainer.style.marginTop = `${heightDiff}px`;
        } else {
            // Fallback (might not work as well)
            chatContainer.style.height = `${window.innerHeight}px`;
        }

        // Setting scrollTop doesn't work right after adjusting marginTop in mobile safari, so
        // we need to wait for the next frame and scroll to the latest message
        requestAnimationFrame(() => {
            // Scroll to the latest visible message
            const lastMessage = messagesContainer.lastElementChild;
            if (lastMessage) {
                const containerRect = messagesContainer.getBoundingClientRect();
                const messageRect = lastMessage.getBoundingClientRect();

                if (messageRect.bottom > containerRect.bottom) {
                    lastMessage.scrollIntoView({ behavior: 'instant', block: 'end' });
                }
            }
        });
    }

    // Send the current contents of the user message
    function sendMessage() {
        const message = {
            content: userMessage,
            user: true
        };
        addMessage(message);
        userMessage = '';
        messageInput.focus();

        // Mock reply from a server
        setTimeout(() => {
            const message = {
                content: 'Hello, world!',
                user: false
            };
            addMessage(message);
        }, 500);
    }

    // Add a message to the messages array and scroll to the bottom
    function addMessage(message: { content: string; user: boolean }) {
        messages.push(message);
        messages = messages;
        requestAnimationFrame(() => {
            const scrollTarget = messagesContainer.scrollHeight - messagesContainer.clientHeight;
            messagesContainer.scrollTo({
                top: scrollTarget,
                behavior: 'smooth'
            });
        });
    }

    // Hella messages for testing
    let messages: { content: string; user: boolean }[] = [
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        },
        {
            content: 'Hello, world!',
            user: false
        },
        {
            content: 'Hello, world!',
            user: true
        }
    ];
</script>

<div class="chat-container" bind:this={chatContainer}>
    <div class="messages-container" bind:this={messagesContainer}>
        <div class="top-message-gap"></div>
        {#each messages as message}
            <div class="message" class:user={message.user}>
                <div class="message-content">{message.content}</div>
            </div>
        {/each}
    </div>
    <div class="input-container" bind:this={inputContainer}>
        <textarea
            class="message-input"
            rows="1"
            placeholder="Message..."
            bind:this={messageInput}
            bind:value={userMessage}
            on:keypress={(event) => {
                if (event.key === 'Enter') {
                    event.preventDefault();
                    sendMessage();
                }
            }}
        ></textarea>
        <button class="send-button" on:click={sendMessage}>Send</button>
    </div>
</div>

<style>
    :global(body) {
        overflow: hidden;
        position: fixed;
        width: 100%;
        height: 100%;
    }

    .chat-container {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        display: flex;
        flex-direction: column;
        overflow: hidden;
        height: 100vh; /* fallback */
        height: calc(var(--vh, 1vh) * 100);
    }

    .messages-container {
        flex-grow: 1;
        overflow-y: auto;
        display: flex;
        flex-direction: column;
        padding: 0.5rem;
        padding-bottom: 0;
        overscroll-behavior: contain;
    }

    .top-message-gap {
        flex-grow: 1;
    }

    .message {
        display: flex;
        flex-direction: row;
        align-items: flex-start;
        padding-bottom: 0.5rem;
    }

    .message.user {
        flex-direction: row-reverse;
    }

    .message-content {
        margin: 0;
        border-radius: 0.5rem;
        padding: 0.5rem;
        background-color: #f0f0f0;
        max-width: 70%;
        word-wrap: break-word;
    }

    .message.user .message-content {
        background-color: #007bff;
        color: white;
    }

    .input-container {
        flex-shrink: 0;
        padding: 0.5rem;
        background-color: #f0f0f0;
        display: flex;
        gap: 0.5rem;
    }

    .message-input {
        flex-grow: 1;
        padding: 0.5rem;
        border: 1px solid #ccc;
        border-radius: 0.5rem;
        resize: none;
    }

    .send-button {
        padding: 0.5rem 1rem;
        background-color: #007bff;
        color: white;
        border: none;
        border-radius: 0.5rem;
        cursor: pointer;
    }
</style>
