<script>
  import axios from "axios";

  const api_root = "https://ganesrakzhaw.pythonanywhere.com/question";

  let messages = {
    question: null,
    answer: null,
  };
  let chat = [];
  let sendingQuestion = {
    question: null,
    sessionId: Math.floor(Math.random() * 10000000000),
  };

  function welcomeMessage() {
    chat.push(
      "A:" +
        "Hi, ich bin ein Chatbot, der dir die Fragen zum Studiengang Betriebsökonomie und seine Vertiefungen beantworten kann. Stelle deine Frage!"
    );
  }

  welcomeMessage();

  function sendMessage() {
    chat.push("Q:" + sendingQuestion.question);
    chat = chat;
    console.log(chat);
    console.log(sendingQuestion);
    var config = {
      method: "post",
      url: api_root,
      headers: {
        "Content-Type": "application/json",
      },
      data: sendingQuestion,
    };
    axios(config)
      .then(function (response) {
        messages = response.data;
        chat.push("A:" + messages.lastAnswer);
        sendingQuestion.question = "";
        console.log(messages.lastAnswer);
        chat = chat;
      })
      .catch(function (error) {
        alert("Error, please reload the page");
        console.log(error);
      });
    sendingQuestion.question = "";
  }

  var input = document.getElementById("message");
  if (input)
  input.addEventListener("keypress", function (event) {
    if (event.key === "Enter") {
      document.getElementById("send").click();
    }
  });
  
  function handleKeyPress(event) {
    if (event.key === "Enter") {
      event.preventDefault();
      sendingQuestion.question = sendingQuestion.question;
      sendMessage();
    }
  }

</script>

<div class="card mx-auto mt-5" style="width: 450px; min-height: 550px;">
  <div
    class="card-header text-center"
    style="background-color: #0064A6; color: white;"
  >
    BO-Chatbot
  </div>
  <div class="card-body" style="background-color: #C5E8FF;">
    <div class="overflow-auto">
      {#each chat as c}
        {#if c.startsWith("A:")}
          <div
            class="d-flex p-2"
            style="display: flex; justify-content: flex-start"
          >
            <!-- svelte-ignore a11y-missing-attribute -->
            <img
              src="Robot.png"
              width="30"
              height="30"
              style="align-self: center; margin-right: 10pt; margin-left: -5pt;"
            />
            <div class="chatbot ml-2 p-3">
              {c.slice(2)}
            </div>
          </div>
        {:else if c.startsWith("Q:")}
          <div
            class="d-flex p-2"
            style="display: flex; justify-content: flex-end"
          >
            <div class="human mr-2 p-3">
              {c.slice(2)}
            </div>
            <!-- svelte-ignore a11y-missing-attribute -->
            <img
              src="Student.png"
              width="30"
              height="30"
              style="align-self: center; margin-left: 10pt; margin-right: -5pt;"
            />
          </div>
        {/if}
      {/each}
    </div>
  </div>
  <div class="card-footer">
    <form class="row g-3">
      <div class="col-10">
        <input
          class="form-control form-control-lg"
          type="text"
          id="message"
          placeholder="Stelle deine Frage..."
          aria-label="default input example"
          bind:value={sendingQuestion.question}
          on:keyup={handleKeyPress}
        />
      </div>
      <div class="col-2">
        <button
          type="button"
          class="btn"
          style="background-color: #C5E8FF; margin-top: 5px;"
          on:click={sendMessage}
          id="send"
        >
          <img
            width="24"
            height="24"
            src="https://img.icons8.com/material-rounded/24/sent.png"
            alt="sent"
          />
        </button>
      </div>
    </form>
  </div>
</div>
