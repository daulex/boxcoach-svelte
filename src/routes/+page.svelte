<script>
  const punch_names = ['', 'jab', 'cross', 'left hook', 'right hook', 'left uppercut', 'right uppercut'];
  let punches = 2;
  let number_of_reps = 5;
  let number_of_rounds = 4;

  let no_doubles = false;
  let alternate_arm = true;
  let each_round_increases_punches = true;

  let combination = [];
  let reps = [];
  let rounds = [];
  
  

  let left_arm = [1,3,5];
  let right_arm = [2,4,6];

  const generateRound = (increase_by = 0) => {
    let punches_internal = punches;
    if(increase_by){
      punches_internal = punches + increase_by;
    }
  combination = [];
    for(let i = 0; i < punches_internal; i++) {
      let newNumber = Math.floor(Math.random() * 6) + 1;
      if(i !== 0){

        if(alternate_arm) {
          // next punch must use different arm from previous punch
          if(left_arm.includes(newNumber) && left_arm.includes(combination[i-1])) {
            newNumber = right_arm[Math.floor(Math.random() * 3)];
          } else if(right_arm.includes(newNumber) && right_arm.includes(combination[i-1])) {
            newNumber = left_arm[Math.floor(Math.random() * 3)];
          }
        }else{
          if(no_doubles && newNumber === combination[i-1]) {
            newNumber = newNumber === 6 ? 1 : newNumber + 1;
          }
        }

      }
      combination.push(newNumber);
    }
    
  };

  const generateReps = (increase = 0) => {
    reps = [];

    for(let i = 0; i < number_of_reps; i++) {
      
      generateRound(increase);
      if(each_round_increases_punches) {
        
      }else{
        generateRound();
      }
      reps.push(combination);

    }
    return reps;
    
  };

  const generateRounds = () => {
    rounds = [];
    for(let i = 0; i < number_of_rounds; i++) {
      rounds.push(generateReps(each_round_increases_punches ? i : 0));
    }
    console.log(rounds);
  };

  

</script>


  
  

<form on:submit={generateRounds}>
  <div class="row">
    <label for="number">Number of punches in first round</label>
    <input id="number" type="number" bind:value={punches}>
  </div>

  <div class="row">
    <label for="number">Number of reps</label>
    <input id="number" type="number" bind:value={number_of_reps}>
  </div>

  <div class="row">
    <label for="number">Number of rounds</label>
    <input id="number" type="number" bind:value={number_of_rounds}>
  </div>

  <div class="row">
    <label for="no_doubles">
      <input id="no_doubles" type=checkbox bind:checked={no_doubles}>
      <span>force no doubles</span>
    </label>
  </div>
  <div class="row">
    <label for="alternate_arm">
      <input id="alternate_arm" type=checkbox bind:checked={alternate_arm}>
      <span>alternate arms</span>
    </label>
  </div>

  <div class="row">
    <label for="each_round_increases_punches">
      <input id="each_round_increases_punches" type=checkbox bind:checked={each_round_increases_punches}>
      <span>each round increases punches</span>
    </label>
  </div>

  <button>Generate reps</button>
</form>






{#if rounds.length}
{#each rounds as round, i}
<div class="round">
  <h2>Round {i+1}</h2>
  {#each round as rep, i}
    <ul>
      <li>
        <strong>
          Rep {i+1}: 
        </strong>
    {#each rep as punch}
      
        <div class="punch">
          <strong>{punch}</strong>
         <em>{punch_names[punch]}</em>
        </div>
      
    {/each}
     </li>
    </ul>
  {/each}
</div>
{/each}

{/if}





<style>
  .punch{
    display: inline;
  }
  .punch::after{
    content: ',';
    display: inline-block;
    margin-right: 10px;
  }
  .punch:last-child::after{
    content: '';
  }
</style>