<script lang="ts">
  import { Route, Router } from "svelte-routing";
  export const url = "/";

  import type { AuthSession } from "@supabase/supabase-js";
  import { onMount } from "svelte";
  import Auth from "./lib/Auth.svelte";
  import Board from "./lib/Board.svelte";
  import Header from "./lib/Header.svelte";
  import { supabase } from "./supabaseClient";

  let session: AuthSession;
  let loading = true;

  onMount(() => {
    supabase.auth.getSession().then(({ data }) => {
      session = data.session;
      loading = false;
    });

    supabase.auth.onAuthStateChange((_event, _session) => {
      session = _session;
    });
  });
</script>

<Router>
  <Route path="/">
    <div class="container" style="padding: 50px 0 100px 0">
      <Header {session} {loading} />
      <Board {session} />
    </div>
  </Route>
  <Route path="/auth">
    <Auth />
  </Route>
</Router>
