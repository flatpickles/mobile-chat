<script lang="ts">
	import { afterUpdate, onMount } from 'svelte';

	let messageInput: string = '';
	let messagesContainer: HTMLElement;
	let inputContainer: HTMLElement;

	onMount(() => {
		const updateVh = () => {
			let vh = window.innerHeight * 0.01;
			document.documentElement.style.setProperty('--vh', `${vh}px`);
		};

		updateVh();

		window.addEventListener('resize', updateVh);
		window.addEventListener('orientationchange', updateVh);

		if (messagesContainer) {
			messagesContainer.addEventListener('touchstart', handleTouchStart, { passive: false });
			messagesContainer.addEventListener('touchmove', handleTouchMove, { passive: false });
		}

		if (inputContainer) {
			inputContainer.addEventListener('touchmove', preventScroll, { passive: false });
		}

		return () => {
			window.removeEventListener('resize', updateVh);
			window.removeEventListener('orientationchange', updateVh);

			if (messagesContainer) {
				messagesContainer.removeEventListener('touchstart', handleTouchStart);
				messagesContainer.removeEventListener('touchmove', handleTouchMove);
			}

			if (inputContainer) {
				inputContainer.removeEventListener('touchmove', preventScroll);
			}
		};
	});

	afterUpdate(() => {
		if (messagesContainer) {
			messagesContainer.scrollTop = messagesContainer.scrollHeight;
		}
	});

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

	function sendMessage() {
		const message = {
			content: messageInput,
			user: true
		};
		messages.push(message);
		messageInput = '';
		messages = messages;

		setTimeout(() => {
			const message = {
				content: 'Hello, world!',
				user: false
			};
			messages.push(message);
			messages = messages;
		}, 500);
	}

	function handleKeyPress(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			event.preventDefault();
			sendMessage();
		}
	}

	function handleTouchStart(event: TouchEvent) {
		console.log('touchstart', event);
		if (inputContainer && inputContainer.contains(event.target as Node)) {
			event.preventDefault();
		}
	}
	function handleTouchMove(event: TouchEvent) {
		console.log('touchmove', event);
		if (inputContainer && inputContainer.contains(event.target as Node)) {
			event.preventDefault();
		}
	}

	function preventScroll(event: TouchEvent) {
		event.preventDefault();
	}
</script>

<div class="chat-container">
	<div class="messages-container" bind:this={messagesContainer}>
		{#each messages as message}
			<div class="message" class:user={message.user}>
				<div class="message-content">{message.content}</div>
			</div>
		{/each}
	</div>
	<div class="input-container" bind:this={inputContainer}>
		<textarea class="message-input" rows="1" bind:value={messageInput} on:keypress={handleKeyPress}
		></textarea>
		<button class="send-button" on:click={sendMessage}>Send</button>
	</div>
</div>

<style>
	:global(:root) {
		--vh: 1vh;
	}

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
		bottom: 0;
		height: 100vh; /* fallback */
		height: calc(var(--vh, 1vh) * 100);
		display: flex;
		flex-direction: column;
		overflow: hidden;
	}

	.messages-container {
		flex-grow: 1;
		overflow-y: auto;
		-webkit-overflow-scrolling: touch;
		display: flex;
		flex-direction: column;
		overscroll-behavior: contain;
	}

	.message {
		display: flex;
		flex-direction: row;
		align-items: flex-start;
		margin: 1rem;
		margin-top: 1rem;
	}

	.message.user {
		flex-direction: row-reverse;
	}

	.message-content {
		margin: 0;
		border-radius: 0.5rem;
		padding: 1rem;
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
		padding: 10px;
		background-color: #f0f0f0;
		display: flex;
		position: sticky;
		bottom: 0;
	}

	.message-input {
		flex-grow: 1;
		margin-right: 10px;
		padding: 5px;
		border: 1px solid #ccc;
		border-radius: 4px;
		resize: none;
	}

	.send-button {
		padding: 5px 10px;
		background-color: #007bff;
		color: white;
		border: none;
		border-radius: 4px;
		cursor: pointer;
	}

	@media (max-width: 600px) {
		.message-content {
			max-width: 85%;
		}
	}
</style>
