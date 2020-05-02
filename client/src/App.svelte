<script>
  import { Router, Link, Route } from "svelte-routing";
  import Rooms from "./Rooms.svelte";
  import Home from "./Home.svelte";

  export let url = "";
</script>

<Router {url}>
  <nav>
    <Link to="/">Home</Link>
    <Link to="/rooms">Rooms</Link>
  </nav>
  <div>
    <Route path="/rooms" component={Rooms} />
    <Route path="/">
      <Home />
    </Route>
  </div>
</Router>
