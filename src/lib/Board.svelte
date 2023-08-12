<script lang="ts">
  import type { AuthSession } from "@supabase/supabase-js";
  import { supabase } from "../supabaseClient";

  export let session: AuthSession;

  let fetchLoading = false;
  let saveLoading = false;
  let cause: string | null = null;
  let effect: string | null = null;
  let solutions: string[] | null = null;
  let updatedAt: string | null = null;
  let listItems = [""];
  let inputSolutions = [];

  $: if (session) {
    fetchData();
  }

  $: if (solutions) {
    listItems = solutions;
  }

  async function fetchData() {
    try {
      fetchLoading = true;
      let { user } = session;

      const { data, error, status } = await supabase
        .from("lists")
        .select("cause, effect, solutions, updated_at")
        .eq("user_id", user.id)
        .single();

      if (error && status !== 406) throw error;

      if (!data) {
        const { error } = await supabase
          .from("lists")
          .insert({ user_id: user.id });

        if (error) throw error;
      } else {
        cause = data.cause;
        effect = data.effect;
        solutions = data.solutions;
        updatedAt = data.updated_at;
      }
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message);
      }
    } finally {
      fetchLoading = false;
    }
  }

  function addMore() {
    listItems.push("");
    listItems = listItems;
  }

  function deleteOne() {
    if (listItems.length === 1) return;
    listItems.pop();
    listItems = listItems;

    inputSolutions.pop();
    inputSolutions = inputSolutions;
  }

  async function saveData() {
    inputSolutions = inputSolutions.filter(Boolean);
    solutions = inputSolutions.map((sol) => sol?.value);

    try {
      saveLoading = true;

      const { user } = session;
      const { data, error } = await supabase
        .from("lists")
        .update({
          cause,
          effect,
          solutions,
          updated_at: new Date().toISOString(),
        })
        .eq("user_id", user.id)
        .select("updated_at");

      const { updated_at } = data[0];
      updatedAt = updated_at;
      if (error) throw error;
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message);
      }
    } finally {
      saveLoading = false;
    }
  }
</script>

<div class="container">
  <div class="mt-5 flex gap-2 items-center">
    <p>If</p>
    <input
      class="w-28"
      type="text"
      placeholder="type here..."
      bind:value={cause}
    />
    <p>were to be</p>
    <input
      class="w-28"
      type="text"
      placeholder="type here..."
      bind:value={effect}
    />
    <p>I'd need to:</p>
  </div>

  <ul class="ml-5 mt-5 list-decimal flex flex-col gap-2">
    {#if fetchLoading}
      <p>⏳ loading...</p>
    {:else}
      {#each listItems as item, index}
        <li>
          <input
            class="w-full"
            type="text"
            placeholder="type here..."
            value={item ?? ""}
            bind:this={inputSolutions[index]}
          />
        </li>
      {/each}
      <li>
        <button on:click={addMore}> 🌱 Add more</button>
        <button on:click={deleteOne}>🔥 Delete one</button>
      </li>
    {/if}
  </ul>
</div>

<div class="container">
  <hr class="w-full h-1 my-8 border-dashed border-gray-600" />
  <div class="mt-4 flex gap-2 items-center">
    <button
      class="button primary"
      on:click={saveData}
      disabled={saveLoading || !session}>💾 Save</button
    >
    {#if !session}
      <p>Sign in to save your data</p>
    {:else if saveLoading}
      <p>⏳ Saving now...</p>
    {:else}
      <p>
        Last save at:
        <time datetime={updatedAt}>
          {new Date(updatedAt).toLocaleString()}
        </time>
      </p>
    {/if}
  </div>
</div>
