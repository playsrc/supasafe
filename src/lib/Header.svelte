<script lang="ts">
  import { onMount } from "svelte";
  import type { AuthSession } from "@supabase/supabase-js";
  import { supabase } from "../supabaseClient";

  export let session: AuthSession;

  const email = "email@example.com";

  // let loading = false;
  // let username: string | null = null;
  // let website: string | null = null;
  // let avatarUrl: string | null = null;

  // onMount(() => {
  //   getProfile();
  // });

  // const getProfile = async () => {
  //   try {
  //     loading = true;
  //     const { user } = session;

  //     const { data, error, status } = await supabase
  //       .from("profiles")
  //       .select("username, website, avatar_url")
  //       .eq("id", user.id)
  //       .single();

  //     if (error && status !== 406) throw error;

  //     if (data) {
  //       username = data.username;
  //       website = data.website;
  //       avatarUrl = data.avatar_url;
  //     }
  //   } catch (error) {
  //     if (error instanceof Error) {
  //       alert(error.message);
  //     }
  //   } finally {
  //     loading = false;
  //   }
  // };
</script>

<div class="container">
  <h1 class="text-4xl leading-relaxed font-bold">Supasafe 🛡</h1>

  <div class="flex gap-2 items-center">
    <p>Signed in as <span class="font-bold">{email}</span></p>
    <button type="button" on:click={() => supabase.auth.signOut()}>
      Sign Out
    </button>
  </div>
  <hr class="w-full h-1 my-4 border-dashed border-gray-600" />
</div>
