<svelte:head>
  <title>WireGuard VPN</title>
</svelte:head>

<script>
  import { onMount } from 'svelte';
  import { Router, Link, Route } from "svelte-routing";
  import About from "./About.svelte";
  import Clients from "./Clients.svelte";
  import EditClient from "./EditClient.svelte";
  import Nav from "./Nav.svelte";

  import Cookie from "cookie-universal";
  const cookies = Cookie();
  export let user = cookies.get("wguser", { fromRes: true}) || "anonymous";

  var urlRelativePath = './';
  
  if (document.location.pathname.indexOf("/about") !== -1) {
    urlRelativePath = './about';
  }
  
  if (document.location.pathname.indexOf("/client") !== -1) {
    urlRelativePath = './client';
  }  
  
  export let url = urlRelativePath;
  
</script>

<style>
main {
/*  max-width: 960px;
  margin-left: auto;
  margin-right: auto;
*/

}
footer {
  margin-top: 3em;
  border-top: 1px solid #ddd;
  text-align: center;
  background: #f7f7f7;
}
</style>

<div class="mdc-typography">

  <Router url="{url}" basepath="./">

    <Nav user="{user}"/>

    <main role="main" class="container">
      <div>
        <Route path="/client" component="{EditClient}" />
        <Route path="/about" component="{About}" />
        <Route path="/"><Clients user="{user}" /></Route>
      </div>
    </main>

  </Router>

  <footer>
    <p>
      Powered by <a href="https://github.com/EmbarkStudios/wireguard-ui" target="_blank">WireGuard UI</a>.
    </p>
    <p>
      Copyright &copy; 2019 <a href="https://embark-studios.com" target="_blank">Embark Studios</a>.
    </p>
  </footer>
</div>
