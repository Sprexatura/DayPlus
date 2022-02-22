<script>
  import {GoogleAuth} from '@beyonk/svelte-social-auth/src/components.js'
  import {Datepicker} from "svelte-calendar"

  let user =  {
    loggedIn: false,
    name: '',
    email: '',
  };

  async function getResult() {
    let response = await fetch('http://127.0.0.1:8000');
    let text = await response.text();
    let data = text;
    return data;
  }

  function loginSuccess(e) {
    user.loggedIn = true;
    var my_user = e.detail.user;
    console.dir(e.detail.user);
    //console.dir(Object.keys(my_user));
    user.name = my_user.Ju.sf;
    user.email = my_user.Ju.zv;    
  }

  // Score radio buttion
  let score = 0;

  // Score reason area
  let reason_placeholder = '아침에 일찍 일어나서 운동을 다녀왔어요\n#미라클모닝 #운동';
  let reason = '';
  
</script>


<main>
  {#if !user.loggedIn}
    <GoogleAuth clientId="872907939780-j1es680sa2nmgu3shgf6q123sca0p5oa.apps.googleusercontent.com" on:auth-success={loginSuccess}/>
  {:else}
    <h1>Day + Me</h1>
    <p>안녕하세요 {user.name}({user.email})</p>
    <div class="wrapper">
      <div>
        <Datepicker format='YYYY/MM/DD'/>
      <p>
        오늘 하루 어떠셨나요
      </div>
      <div>
        <div class="score-buttons">
          <label>
            <input type=radio bind:group={score} value={-1}/>-1
          </label>
          <label>
            <input type=radio bind:group={score} value={0}/>0
          </label>
          <label>
            <input type=radio bind:group={score} value={1}/>+1
          </label>
        </div>
      </div>
      <div>
        <textarea class="reason" placeholder={reason_placeholder} bind:value={reason}></textarea>
      </div>
      <div>
        <button class="submit" type="button">입력</button>
      </div>
    </div>
  {/if}
</main>

<style>
  main {
	text-align: center;
	padding: 1em;
	max-width: 240px;
	margin: 0 auto;
  }
    
  h1 {
	color: #ff3e00;
	text-transform: uppercase;
	font-size: 4em;
	font-weight: 100;
  }

  @media (min-width: 640px) {
	main {
	  max-width: none;
	}
  }

  .score-buttons input[type="radio"] {
    width: 15px;
  }

  .score-buttons label {
    display: inline;
    margin-left: 5px;
  }

  .reason { width: 240px; height: 100px; }
</style>
