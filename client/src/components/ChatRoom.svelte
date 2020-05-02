<script>
  var data = {
    ws: null,
    newMsg: "",
    chatContent: "",
    email: null,
    username: null,
    joined: false
  };

  data.ws = new WebSocket("ws://" + "localhost:8000" + "/ws");

  data.ws.addEventListener("message", e => {
    var msg = JSON.parse(e.data);

    console.log(msg);
    console.log(data);

    data.chatContent +=
      '<div class="chip">' +
      '<img src="' +
      gravatarURL(msg.email) +
      '">' + // Avatar
      msg.username +
      "</div>" +
      msg.message;
    "<br/>"; // Parse emojis

    var element = document.getElementById("chat-messages");
    element.scrollTop = element.scrollHeight;
  });

  const send = () => {
    console.log(data);

    if (data.newMsg != "") {
      data.ws.send(
        JSON.stringify({
          email: data.email,
          username: data.username,
          message: data.newMsg
        })
      );
      data.newMsg = "";
    }
  };

  const join = (username, email) => {
    console.log(data);

    if (!data.email) {
      Materialize.toast("You must enter an email", 3000);
      return;
    }

    if (!data.username) {
      Materialize.toast("You must choose a username", 3000);
      return;
    }

    data.joined = true;
  };

  function gravatarURL(email) {
    return "http://www.gravatar.com/avatar/";
  }
</script>

<style>
  body {
    display: flex;
    min-height: 100vh;
    flex-direction: column;
  }

  main {
    flex: 1 0 auto;
  }

  #chat-messages {
    min-height: 10vh;
    height: 60vh;
    width: 100%;
    overflow-y: scroll;
  }
</style>

<h1>ChatRooms</h1>
<header>Simple Chat</header>
<main id="app">
  <div class="row">
    <div class="col s12">
      <div class="card horizontal">
        <div id="chat-messages" class="card-content" v-html="chatContent">
          {@html data.chatContent}
        </div>
      </div>
    </div>
  </div>
  <div class="row" v-if="joined">
    <div class="input-field col s8">
      <input type="text" bind:value={data.newMsg} />
    </div>
    <div class="input-field col s4">
      <button class="waves-effect waves-light btn" on:click={send}>
        <i class="material-icons right">chat</i>
        Send
      </button>
    </div>
  </div>
  {#if data.joined === false}
    <div class="row" v-if="!joined">
      <div class="input-field col s8">
        <input type="email" placeholder="Email" bind:value={data.email} />
      </div>
      <div class="input-field col s8">
        <input type="text" placeholder="Username" bind:value={data.username} />
      </div>
      <div class="input-field col s4">
        <button on:click={join}>
          <i class="material-icons right">done</i>
          Join
        </button>
      </div>
    </div>
  {/if}
</main>
<footer class="page-footer" />
