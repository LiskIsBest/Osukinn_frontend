<script>
  import {onMount} from "svelte";

  import User from "./User.svelte"

  export const queryString = window.location.search;
  export const urlParams = new URLSearchParams(queryString);
  let usernames = urlParams.get("usernames");
  let mode = urlParams.get("mode");
  
  let username_list = usernames.split(",");
  username_list = username_list.map(username => username.trim());

  console.log(username_list);
  console.log(usernames, mode);

  function endpoint(username){
    return `https://www.osukinn.com/users/${username}`
  }

  let user_data = [];

  onMount(async function(){
    username_list.forEach(async (username)=>{
      console.log(`fetching data for user:${username}`)
      const response = await fetch(endpoint(username));
      const data = await response.json();
      console.log(data);
      user_data.push(data);
      user_data = user_data;
    })
  })

  console.log(user_data)
</script>

<main>

<head>
  <title>OsuKinn</title>
  <link rel="icon" type="image/x-icon" href="/assets/favicon.png">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
  <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.2/dist/umd/popper.min.js" ></script>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.1/dist/js/bootstrap.min.js" integrity="sha384-Atwg2Pkwv9vp0ygtn1JAojH0nYbwNJLPhwyoVbhoPwBhjQPR5VtM2+xf0Uwh9KtT" crossorigin="anonymous"></script>
</head>

<body>

  <div class="row d-flex">
    <form class="col-12 text-center" method="get" action="/">
      <label for="usernames">Enter usernames (ex: username1, username2, etc..) </label> <br />
      <div class="d-inline-flex">
        <select name="mode">
          <option value="mania" >Mania</option>
          <option value="osu" >Standard</option>
          <option value="taiko">Taiko</option>
          <option value="fruits">Ctb</option>
          <option value="all">All</option>
        </select>
        <input type="hidden" id="mode">
        <input type="text" id="usernames" name="usernames"><br />
      </div>
    </form>
  </div>

  <div class="d-flex row justify-content-center border border-secondary">
    <h1 class="text-center">User</h1>
    <!-- {#each user_data as user, index}
      <User class="col-md-6 col-sm-6 col-lg-3 border border-primary" user={user} mode={mode}/>
      {/each} -->
    <User class="col-md-6 col-sm-6 col-lg-3 border border-primary" users={user_data} mode={mode}/>
  </div>

  <div>
    <form class="col-12 text-center" method='get' action='/update'>
      <button>refresh users</button>
  </form>
  </div>
</body>

</main>

<style>
</style>
