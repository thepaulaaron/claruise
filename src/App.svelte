<script>
  // Define names and corresponding MBTI types as key-value pairs
  const people = {
    "Alice": "INFP",
    "Bob": "ENFP",
    "Charlie": "INTJ",
    "Diana": "ENTP",
    "Eve": "ISFJ",
    "Frank": "ESTJ",
    // Add more people as needed
  };

  // Define MBTI types (to determine index in match levels)
  const mbtiTypes = [
    "INFP", "ENFP", "INFJ", "ENFJ",
    "INTJ", "ENTJ", "INTP", "ENTP",
    "ISFP", "ESFP", "ISTP", "ESTP",
    "ISFJ", "ESFJ", "ISTJ", "ESTJ"
  ];

  // Define match levels (1 = Disaster, 2 = Bad, 3 = Okay, 4 = Good, 5 = Perfect)
  const matchLevels = [
    [4, 4, 4, 5, 4, 5, 4, 4, 1, 1, 1, 1, 1, 1, 1, 1], // INFP
    [4, 4, 5, 4, 5, 4, 4, 4, 1, 1, 1, 1, 1, 1, 1, 1], // ENFP
    [4, 5, 4, 4, 4, 4, 4, 5, 1, 1, 1, 1, 1, 1, 1, 1], // INFJ
    [5, 4, 4, 4, 4, 4, 4, 4, 5, 1, 1, 1, 1, 1, 1, 1], // ENFJ
    [4, 5, 4, 4, 4, 4, 4, 5, 3, 3, 3, 3, 2, 2, 2, 2], // INTJ
    [5, 4, 4, 4, 4, 4, 5, 4, 3, 3, 3, 3, 3, 3, 3, 3], // ENTJ
    [4, 4, 4, 4, 4, 5, 4, 4, 3, 3, 3, 3, 3, 3, 3, 3], // INTP
    [4, 4, 5, 4, 5, 4, 4, 4, 3, 3, 3, 3, 2, 2, 2, 2], // ENTP
    [1, 1, 1, 5, 3, 3, 3, 3, 2, 2, 2, 2, 3, 5, 3, 5], // ISFP
    [1, 1, 1, 1, 3, 3, 3, 3, 2, 2, 2, 2, 5, 3, 5, 3], // ESFP
    [1, 1, 1, 1, 3, 3, 3, 3, 2, 2, 2, 2, 3, 5, 3, 5], // ISTP
    [1, 1, 1, 1, 3, 3, 3, 3, 2, 2, 2, 2, 5, 3, 5, 3], // ESTP
    [1, 1, 1, 1, 2, 3, 2, 2, 3, 5, 3, 5, 4, 4, 4, 4], // ISFJ
    [1, 1, 1, 1, 2, 3, 2, 2, 5, 3, 5, 3, 4, 4, 4, 4], // ESFJ
    [1, 1, 1, 1, 2, 3, 2, 2, 3, 5, 3, 5, 4, 4, 4, 4], // ISTJ
    [1, 1, 1, 1, 2, 3, 5, 2, 5, 3, 5, 3, 4, 4, 4, 4], // ESTJ
  ];

  // Input and output variables
  let person1 = "";
  let person2 = "";
  let result = "";

  // Interpret match levels
  function interpretMatchLevel(level) {
    return ["Disaster", "Bad", "Okay", "Good", "Perfect"][level - 1];
  }

  // Calculate match level
  function calculateCompatibility() {
    const mbti1 = people[person1];
    const mbti2 = people[person2];

    if (!mbti1 || !mbti2) {
      result = "Invalid person selected!";
      return;
    }

    const index1 = mbtiTypes.indexOf(mbti1);
    const index2 = mbtiTypes.indexOf(mbti2);

    if (index1 === -1 || index2 === -1) {
      result = "Invalid MBTI type found!";
      return;
    }

    const level = matchLevels[index1][index2];
    result = `Match level between ${person1} (${mbti1}) and ${person2} (${mbti2}): ${interpretMatchLevel(level)}`;
  }
</script>

<style>
  .container {
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
    text-align: center;
  }

  select, button {
    margin: 10px 0;
    padding: 10px;
    font-size: 16px;
  }
</style>

<div class="container">
  <h1>MBTI Compatibility Checker</h1>

  <label for="person1">Select Person 1:</label>
  <select id="person1" bind:value={person1}>
    <option value="" disabled selected>Choose...</option>
    {#each Object.keys(people) as name}
      <option value={name}>{name}</option>
    {/each}
  </select>

  <label for="person2">Select Person 2:</label>
  <select id="person2" bind:value={person2}>
    <option value="" disabled selected>Choose...</option>
    {#each Object.keys(people) as name}
      <option value={name}>{name}</option>
    {/each}
  </select>

  <button on:click={calculateCompatibility}>Check Compatibility</button>

  {#if result}
    <p>{result}</p>
  {/if}
</div>
