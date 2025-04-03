<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";

  // let profileData = fetch("https://api.github.com/users/juan1t0");
</script>

<svelte:head>
  <title>Leiton Estrada R: Personal site and portfolio</title>
</svelte:head>
<h1> Leiton Estrada</h1>
   
<img src="./images/Imagen001.jpg" alt="mike" width="500px">

<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
   Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
    Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
     Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
</p>
{#await fetch("https://api.github.com/users/leandr0er")}
  <span>Loading...</span>
{:then response}
  {#await response.json()}
    <span>Decoding...</span>
  {:then data} 
    <section>
      <h2>My Github Stats</h2>
      <dl>
        <dt>Followers</dt>
        <dd>{data.followers}</dd>
        <dt>Following</dt>
        <dd>{data.following}</dd>
        <dt>Public Repos</dt>
        <dd>{data.public_repos}</dd>
      </dl>
    </section>
  {:catch error}
    <span class="error">Something went wrong: {error.message}</span>
  {/await}
  {:catch error}
    <span class="error">Something went wrong: {error.message}</span>
{/await}

<h2>
  Latest Projects
</h2>
<div class="projects">
{#each projects.slice(0, 3) as p}
  <Project data={p} hLevel="3"/>
{/each}
</div>

<style>
  dl{
    display: grid;
    grid-template-columns: auto;
  }
  dt{
    grid-row: 1;
    font-family: inherit;
    font-weight: bold;
    color: var(--border-gray);
    text-transform: uppercase;
  }
  dd{
    font-family: inherit;
    font-weight: bold;
  }
  section{
    border-width:0.15em;
  	border-style:solid;
	  border-color:var(--border-gray);
    padding-left: 1em;
    padding-right: 1em;
  }
</style>