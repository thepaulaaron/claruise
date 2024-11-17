<script>
  // Define names and corresponding MBTI types as key-value pairs
  const people = {
    "Xyllie": "ESFP",
    "Pen": "INFJ",
    "Clyde": "ISTJ",
		"Nikki": "INFP",
		"Bdog": "INTP",
		"Abi": "INFJ",
		"Chichut": "ENFJ",
		"Zy": "ENFP",
		"MJ": "INTJ",
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

  // Generate the compatibility matrix
  const compatibilityMatrix = Object.keys(people).map(person1 => {
    return Object.keys(people).map(person2 => {
      const mbti1 = people[person1];
      const mbti2 = people[person2];

			if (mbti1==mbti2) {
				return "";
			}

      const index1 = mbtiTypes.indexOf(mbti1);
      const index2 = mbtiTypes.indexOf(mbti2);

      const level = matchLevels[index1][index2];
      return interpretMatchLevel(level);
    });
  });

  // Interpret match levels
  function interpretMatchLevel(level) {
    return ["Disaster", "Bad", "Okay", "Good", "Perfect"][level - 1];
  }
</script>

<style>
  .container {
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    text-align: center;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
  }

  table, th, td {
    border: 1px solid black;
  }

  th, td {
    padding: 10px;
    text-align: center;
  }

  th {
    background-color: #f0f0f0;
  }

  td {
    background-color: #fafafa;
  }

  td:hover {
    background-color: #f5f5f5;
  }
</style>

<div class="container">
  <h1>MBTI Compatibility Matrix</h1>
	<p>Made by Claruise (Clyde Aaron Louise)</p>
  <table>
    <thead>
      <tr>
        <th>Person</th>
        {#each Object.keys(people) as person}
          <th>{person}</th>
        {/each}
      </tr>
    </thead>
    <tbody>
      {#each Object.keys(people) as person1, rowIndex}
        <tr>
          <td>{person1}</td>
          {#each compatibilityMatrix[rowIndex] as match, colIndex}
            <td>{match}</td>
          {/each}
        </tr>
      {/each}
    </tbody>
  </table>
</div>
