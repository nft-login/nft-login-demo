<script>
	import { Highlight } from 'svelte-highlight';
	import { json } from 'svelte-highlight/languages';
	import { arduinoLight as highlightTheme } from 'svelte-highlight/styles';
	import {
		OidcContext,
		LoginButton,
		LogoutButton,
		NFTButton,
		RefreshTokenButton,
		authError,
		idToken,
		accessToken,
		isAuthenticated,
		isLoading,
		userInfo,
	} from './components/components.module.js';

	let show_game;

	const unsubscribe = isAuthenticated.subscribe((value) => {
		show_game = value;
	});
</script>

<svelte:head>
  {@html highlightTheme}
</svelte:head>

<div class="container">
	<OidcContext
		issuer="process.env.OIDC_ISSUER"
		client_id="process.env.OIDC_CLIENT_ID"
		redirect_uri="process.env.OIDC_REDIRECT_URI"
		post_logout_redirect_uri="process.env.OIDC_POST_LOGOUT_REDIRECT_URI"
	>
		<LoginButton>Login</LoginButton>
		<LogoutButton>Logout</LogoutButton>
		<RefreshTokenButton>refreshToken</RefreshTokenButton>
		<NFTButton>Get NFT for Login</NFTButton>

		{#if show_game}
			<div>
				<iframe
					title="embedded game"
					src="https://hextris.io/"
					name="hextris"
					width="800"
					height="600"
					frameborder="0"
					scrolling="no"
				/>
				<a href="https://hextris.io/">hextris.io</a>
			</div>
		{/if}

		<table>
			<thead>
				<tr
					><th style="width: 20%;">store</th><th style="width: 80%;"
						>value</th
					></tr
				>
			</thead>
			<tbody>
				<tr><td>isLoading</td><td>{$isLoading}</td></tr>
				<tr><td>isAuthenticated</td><td>{$isAuthenticated}</td></tr>
				<tr
					><td>accessToken</td><td style="word-break: break-all;"
						>{$accessToken}</td
					></tr
				>
				<tr
					><td>idToken</td><td style="word-break: break-all;"
						>{$idToken}</td
					></tr
				>
				<tr
					><td>userInfo</td><td
						><Highlight
							language={json}
							code={JSON.stringify($userInfo, null, 2) || ''}
						/></td
					></tr
				>
				<tr><td>authError</td><td>{$authError}</td></tr>
			</tbody>
		</table>
	</OidcContext>
</div>
