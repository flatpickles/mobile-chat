<script lang="ts">
    import { afterUpdate, onMount } from 'svelte';

    let userMessage: string = '';
    let messageInput: HTMLTextAreaElement;
    let chatContainer: HTMLElement;
    let messagesContainer: HTMLElement;
    let inputContainer: HTMLElement;

    onMount(() => {
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
        } else {
            window.addEventListener('resize', adjustViewportHeight);
            window.addEventListener('orientationchange', adjustViewportHeight);
        }

        // Set viewport height on load
        adjustViewportHeight();

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

    afterUpdate(() => {
        // Scroll to the bottom of the messages container after the messages are updated
        if (messagesContainer) {
            messagesContainer.scrollTo({
                top: messagesContainer.scrollHeight,
                behavior: 'smooth'
            });
        }
    });

    function adjustViewportHeight() {
        if (window.visualViewport) {
            const viewportHeight = window.visualViewport.height;
            chatContainer.style.height = `${viewportHeight}px`;
            window.scrollTo(0, 0);
            messagesContainer.scrollTo({
                top: messagesContainer.scrollHeight,
                behavior: 'instant'
            });
        } else {
            chatContainer.style.height = `${window.innerHeight}px`;
        }
    }

    // Send the current contents of the user message
    function sendMessage() {
        const message = {
            content: userMessage,
            user: true
        };
        messages.push(message);
        messages = messages;
        userMessage = '';
        messageInput.focus();

        // Mock reply from the server
        setTimeout(() => {
            const message = {
                content: 'Hello, world!',
                user: false
            };
            messages.push(message);
            messages = messages;
        }, 500);
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
    }

    .messages-container {
        flex-grow: 1;
        overflow-y: auto;
        display: flex;
        flex-direction: column;
        gap: 0.5rem;
        padding: 0.5rem;
        overscroll-behavior: contain;
    }

    .message {
        display: flex;
        flex-direction: row;
        align-items: flex-start;
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
