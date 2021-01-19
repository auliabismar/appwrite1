<script>
  import Appwrite from "appwrite";
  import { onMount } from "svelte";
  import { active, currentUser } from "../stores";

  const SDK = {
    sdk: new Appwrite(),
    setConfig: config => {
      SDK.sdk
        .setEndpoint(config.endpoint)
        .setProject(config.project)
        .setLocale(config.locale);
    },
  };
  
  onMount(() => {
    try {
      SDK.setConfig({ endpoint: "http://localhost:10443/v1", project: "600259e2a4b1d", locale: "en" });
    } catch (error) {
      console.log(error)
    }
  });

  const fetchUser = async () => {
    try {
      const response = await SDK.sdk.account.get();
      $currentUser = response;
      return response;
    } catch (error) {
      throw error;
    }
  };
  let request = fetchUser();
  </script>

{#if $active}
  {#await request}
    loading
  {:then user} 
    {user}
  {:catch error}
    {error}
  {/await}
{/if}