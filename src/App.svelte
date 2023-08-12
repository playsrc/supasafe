<script lang="ts">
  import { Route, Router } from "svelte-routing";
  export const url = "/";

  import type { AuthSession } from "@supabase/supabase-js";
  import { onMount } from "svelte";
  import Auth from "./lib/Auth.svelte";
  import Board from "./lib/Board.svelte";
  import Footer from "./lib/Footer.svelte";
  import Header from "./lib/Header.svelte";
  import { supabase } from "./supabaseClient";

  let session: AuthSession;

  onMount(() => {
    supabase.auth.getSession().then(({ data }) => {
      session = data.session;
    });

    supabase.auth.onAuthStateChange((_event, _session) => {
      session = _session;
    });
  });
</script>

<Router>
  <Route path="/">
    <div class="container" style="padding: 50px 0 100px 0">
      <Header {session} />
      <Board />
      <Footer {session} />
    </div>
  </Route>
  <Route path="/auth">
    <Auth />
  </Route>
</Router>
