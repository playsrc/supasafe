<script lang="ts">
  import type { AuthSession } from "@supabase/supabase-js";
  import { supabase } from "../supabaseClient";

  export let session: AuthSession;

  let email = "";

  $: email = session?.user?.email || "loading...";
</script>

<div class="container">
  <h1 class="text-4xl leading-relaxed font-bold">Supasafe 🛡</h1>

  {#if !session}
    <div class="flex gap-2 items-center">
      <p>You're currently not signed in</p>
      <a href="/auth" class="button"> 🔑 Sign In </a>
    </div>
  {:else}
    <div class="flex gap-2 items-center">
      <p>Signed in as <span class="font-bold">{email}</span></p>
      <button type="button" on:click={() => supabase.auth.signOut()}>
        ❌ Sign Out
      </button>
    </div>
  {/if}
  <hr class="w-full h-1 my-8 border-dashed border-gray-600" />
</div>
