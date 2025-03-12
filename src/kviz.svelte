<script>
    import { fade } from 'svelte/transition';
  
    let questions = [
      { 
        question: "Jaká planeta je největší ve Sluneční soustavě?", 
        options: ["Mars", "Jupiter", "Neptun", "Země"], 
        correct: 1 
      },
      { 
        question: "Která planeta je nejblíže ke Slunci?", 
        options: ["Venuše", "Země", "Merkur", "Mars"], 
        correct: 2 
      },
      { 
        question: "Která planeta je známá svými prstenci?", 
        options: ["Uran", "Neptun", "Saturn", "Jupiter"], 
        correct: 2 
      },
      { 
        question: "Jak se jmenuje naše galaxie?", 
        options: ["Mléčná dráha", "Andromeda", "Orion", "Velká mlhovina"], 
        correct: 0 
      },
      { 
        question: "Která planeta je nazývána 'rudou planetou'?", 
        options: ["Mars", "Venuše", "Jupiter", "Saturn"], 
        correct: 0 
      },
      { 
        question: "Kolik planet má Sluneční soustava?", 
        options: ["7", "8", "9", "10"], 
        correct: 1 
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
  