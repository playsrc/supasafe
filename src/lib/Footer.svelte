<script lang="ts">
  import type { AuthSession } from "@supabase/supabase-js";
  import { supabase } from "../supabaseClient";

  export let session: AuthSession;

  const savedAt = new Date().toLocaleString();
</script>

<div class="container">
  <hr class="w-full h-1 my-8 border-dashed border-gray-600" />
  <div class="mt-4 flex gap-2 items-center">
    <button class="button primary">💾 Save</button>
    {#if !session}
      <p>Sign in to save your data</p>
    {:else}
      <p>Last save at: <span>{savedAt}</span></p>
    {/if}
  </div>
</div>
