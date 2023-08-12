<script lang="ts">
  import { supabase } from "../supabaseClient";
  import HCaptcha from "svelte-hcaptcha";

  let loading = false;
  let email = "";
  let captchaToken = "";
  let captcha = null;

  const handleLogin = async () => {
    try {
      loading = true;
      const { error } = await supabase.auth.signInWithOtp({
        email,
        options: { captchaToken },
      });
      if (error) throw error;
      alert("Check your email for login link!");
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message);
      }
    } finally {
      loading = false;
      captcha.reset();
    }
  };

  function handleSuccess(event) {
    captchaToken = event.detail.token;
  }

  function handleError(error) {
    captcha.reset();
    console.log("ERROR", error);
  }
</script>

<div class="container row flex-center flex">
  <div class="col-6 form-widget" aria-live="polite">
    <h1 class="text-4xl leading-relaxed font-bold">Supasafe 🛡</h1>
    <p class="description">Sign in via magic link with your email below</p>
    <form class="form-widget" on:submit|preventDefault={handleLogin}>
      <div>
        <label for="email">Email</label>
        <input
          id="email"
          class="inputField"
          type="email"
          placeholder="Your email"
          bind:value={email}
          required
        />
      </div>
      <HCaptcha
        sitekey={import.meta.env.VITE_CAPTCHA_SITE_KEY}
        theme="dark"
        on:success={handleSuccess}
        on:error={handleError}
        bind:this={captcha}
      />
      <div>
        <button
          type="submit"
          class="button block"
          aria-live="polite"
          disabled={loading}
        >
          <span>{loading ? "Loading" : "Send magic link"}</span>
        </button>
      </div>
    </form>
  </div>
</div>
