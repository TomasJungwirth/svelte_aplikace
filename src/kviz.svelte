<script>
  import { fade } from 'svelte/transition';

  let questions = [
    { 
      question: "Jaká planeta je největší ve Sluneční soustavě?", 
      options: ["Venuše", "Jupiter", "Neptun", "Saturn"], 
      correct: 1 
    },
    { 
      question: "Která planeta je ke Slunci nejblíže?", 
      options: ["Venuše", "Země", "Merkur", "Mars"], 
      correct: 2 
    },
    { 
      question: "Která planeta nepatří mezi tzv. plynné obry?", 
      options: ["Uran", "Merkur", "Saturn", "Jupiter"], 
      correct: 1 
    },
    { 
      question: "Jak se jmenuje naše galaxie?", 
      options: ["Mléčná dráha", "Andromeda", "Orion", "Velká mlhovina"], 
      correct: 0 
    },
    { 
      question: "Která planeta je pojmenována po Římském bohu války?", 
      options: ["Mars", "Neptun", "Merkur", "Saturn"], 
      correct: 0 
    },
    { 
      question: "Z kolika planet se skládá naše Sluneční soustava?", 
      options: ["7", "8", "9", "10"], 
      correct: 1 
    },
		 { 
      question: "Kterou z těchto planet nazýváme trpasličí planetou?", 
      options: ["Venuše", "Mars", "Merkur", "Pluto"], 
      correct: 3 
    },
		{ 
      question: "Která planeta je známá díky svým prstencům?", 
      options: ["Venuše", "Saturn", "Uran", "Neptun"], 
      correct: 1 
    },
		{ 
      question: "Slunce je?", 
      options: ["Planeta", "Mlhovina", "Hvězda", "Kometa"], 
      correct: 2 
    },
    { 
      question: "Co je nejjasnější objekt na noční obloze (kromě Měsíce)?", 
      options: ["Polárka", "Venuše", "Mars", "Betelgeuze"], 
      correct: 1 
    }
  ];

  let currentQuestion = 0;
  let score = 0;
  let answered = false;
  let selectedAnswer = null;
  let timeLeft = 10;
  let timer;

  function startTimer() {
    clearInterval(timer);
    timeLeft = 20;
    timer = setInterval(() => {
      if (timeLeft > 0) {
        timeLeft--;
      } else {
        clearInterval(timer);
        nextQuestion();
      }
    }, 1000);
  }

  function selectAnswer(index) {
    if (!answered) {
      answered = true;
      clearInterval(timer);
      selectedAnswer = index;
      if (index === questions[currentQuestion].correct) {
        score++;
      }
      setTimeout(nextQuestion, 1500);
    }
  }

  function nextQuestion() {
    if (currentQuestion < questions.length - 1) {
      currentQuestion++;
      answered = false;
      selectedAnswer = null;
      startTimer();
    } else {
      currentQuestion++;
      clearInterval(timer);
    }
  }

  function restartQuiz() {
    currentQuestion = 0;
    score = 0;
    answered = false;
    selectedAnswer = null;
    startTimer();
  }

  startTimer();
</script>

<main>
  {#if currentQuestion < questions.length}
    <div transition:fade>
      <h2>{questions[currentQuestion].question}</h2>
	          <p>Aktuální skóre: {score} / {questions.length}</p>
      <ul>
        {#each questions[currentQuestion].options as option, i}
          <li>
            <button on:click={() => selectAnswer(i)}
              class:selected={selectedAnswer === i}
              class:correct={answered && i === questions[currentQuestion].correct}
              class:wrong={answered && selectedAnswer === i && i !== questions[currentQuestion].correct}>
              {option}
            </button>
          </li>
        {/each}
      </ul>
			<p>Čas na odpověď: {timeLeft} s</p>
    </div>
  {:else}
    <div transition:fade>
      <h2>Hotovo! Tvoje skóre: {score} / {questions.length}</h2>
      <button on:click={restartQuiz}>Hrát znovu</button>
    </div>
  {/if}
</main>

<style>
  main {
    text-align: center;
    font-family: Arial, sans-serif;
  }
  ul {
    list-style: none;
    padding: 0;
  }
  button {
    display: block;
    margin: 10px auto;
    padding: 10px 20px;
    font-size: 18px;
    border: none;
    cursor: pointer;
  }
	button:hover {
    font-weight: bold;
  }
  .selected {
    background-color: lightgray;
  }
  .correct {
    background-color: green;
    color: white;
  }
  .wrong {
    background-color: red;
    color: white;
  }
</style>
