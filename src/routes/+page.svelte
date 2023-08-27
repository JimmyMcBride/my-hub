<script>
	import { auth, userData } from "$lib/firebase"
	import { GoogleAuthProvider, signInWithPopup, signOut } from "firebase/auth"

	async function signInWithGoogle() {
		const provider = new GoogleAuthProvider()
		const credential = await signInWithPopup(auth, provider)

		const idToken = await credential.user.getIdToken()

		const res = await fetch("/api/signin", {
			method: "POST",
			headers: {
				"Content-Type": "application/json",
				// 'CSRF-Token': csrfToken  // HANDLED by sveltekit automatically
			},
			body: JSON.stringify({ idToken }),
		})
	}

	async function signOutSSR() {
		const res = await fetch("/api/signin", { method: "DELETE" })
		await signOut(auth)
	}
</script>

<svelte:head>
	<title>MyHub.page - a place for links</title>
	<meta name="description" content="Simple hub to share all your links in one place!" />
</svelte:head>

<main class="flex w-full min-h-screen">
	<div class="hero bg-base-200">
		<div class="hero-content text-center">
			<div class="w-full">
				<h1 class="text-7xl font-bold">MyHub.page</h1>
				<p class="py-6 text-2xl">One simple link to share your entire developer portfolio</p>
				{#if $userData?.username}
					<a href="/{$userData.username}/edit" class="btn btn-primary">Edit Profile</a>
					<button class="btn btn-error" on:click={() => signOutSSR()}>Sign out</button>
				{:else}
					<a href="/login" class="btn btn-primary">Get Started</a>
					<a href="/jimmymcbride" class="btn btn-secondary">Example Profile</a>
				{/if}
			</div>
		</div>
	</div>
</main>
