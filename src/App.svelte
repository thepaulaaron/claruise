<script>
    import { onMount } from "svelte";
    import * as d3 from "d3";
  
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
  
    const mbtiTypes = ["INFP", "ENFP", "INFJ", "ENFJ", "INTJ", "ENTJ", "INTP", "ENTP", "ISFP", "ESFP", "ISTP", "ESTP", "ISFJ", "ESFJ", "ISTJ", "ESTJ"];
  
    const matchLevels = [
      [4, 4, 4, 5, 4, 5, 4, 4, 1, 1, 1, 1, 1, 1, 1, 1],
      [4, 4, 5, 4, 5, 4, 4, 4, 1, 1, 1, 1, 1, 1, 1, 1],
      [4, 5, 4, 4, 4, 4, 4, 5, 1, 1, 1, 1, 1, 1, 1, 1],
      [5, 4, 4, 4, 4, 4, 4, 4, 5, 1, 1, 1, 1, 1, 1, 1],
      [4, 5, 4, 4, 4, 4, 4, 5, 3, 3, 3, 3, 2, 2, 2, 2],
      [5, 4, 4, 4, 4, 4, 5, 4, 3, 3, 3, 3, 3, 3, 3, 3],
      [4, 4, 4, 4, 4, 5, 4, 4, 3, 3, 3, 3, 3, 3, 3, 3],
      [4, 4, 5, 4, 5, 4, 4, 4, 3, 3, 3, 3, 2, 2, 2, 2],
      [1, 1, 1, 5, 3, 3, 3, 3, 2, 2, 2, 2, 3, 5, 3, 5],
      [1, 1, 1, 1, 3, 3, 3, 3, 2, 2, 2, 2, 5, 3, 5, 3],
      [1, 1, 1, 1, 3, 3, 3, 3, 2, 2, 2, 2, 3, 5, 3, 5],
      [1, 1, 1, 1, 3, 3, 3, 3, 2, 2, 2, 2, 5, 3, 5, 3],
      [1, 1, 1, 1, 2, 3, 2, 2, 3, 5, 3, 5, 4, 4, 4, 4],
      [1, 1, 1, 1, 2, 3, 2, 2, 5, 3, 5, 3, 4, 4, 4, 4],
      [1, 1, 1, 1, 2, 3, 2, 2, 3, 5, 3, 5, 4, 4, 4, 4],
      [1, 1, 1, 1, 2, 3, 5, 2, 5, 3, 5, 3, 4, 4, 4, 4]
    ];
  
    const nodes = Object.keys(people).map((person, index) => ({
      id: person,
      mbti: people[person],
      group: index,
    }));
  
    const links = [];
Object.keys(people).forEach((person1, index1) => {
  Object.keys(people).forEach((person2, index2) => {
    if (index1 < index2) {
      const mbti1 = people[person1];
      const mbti2 = people[person2];
      const matchIndex1 = mbtiTypes.indexOf(mbti1);
      const matchIndex2 = mbtiTypes.indexOf(mbti2);
      const matchLevel = matchLevels[matchIndex1][matchIndex2];

      // Always connect the nodes (even for low match levels)
      links.push({
        source: person1,
        target: person2,
        value: matchLevel // You can keep the match level for visualization
      });
    }
  });
});
  
    onMount(() => {
      const svg = d3.select("svg");
      const width = 1000;  // Increased width
      const height = 800;  // Increased height
  
      svg.attr("width", width).attr("height", height);
  
      const simulation = d3.forceSimulation(nodes)
        .force("link", d3.forceLink(links).id(d => d.id).distance(400))
        .force("charge", d3.forceManyBody().strength(-200))
        .force("center", d3.forceCenter(width / 2, height / 2));
  
      const link = svg.append("g")
        .selectAll(".link")
        .data(links)
        .enter().append("line")
        .attr("class", d => `link ${getMatchLevelClass(d.value)}`)
        .style("stroke-width", 2)
        .style("stroke", d => getLinkColor(d.value));  // Set the link color here
  
      const node = svg.append("g")
        .selectAll(".node")
        .data(nodes)
        .enter().append("circle")
        .attr("class", "node")
        .attr("r", 10)
        .style("fill", "#69b3a2")
        .call(d3.drag()
          .on("start", dragStarted)
          .on("drag", dragging)
          .on("end", dragEnded));
  
      // Add labels to nodes for person name
      svg.append("g")
        .selectAll(".nodeLabel")
        .data(nodes)
        .enter().append("text")
        .attr("class", "nodeLabel")
        .attr("x", d => d.x)
        .attr("y", d => d.y)
        .attr("dx", 12)
        .attr("dy", 4)
        .text(d => d.id)
        .style("font-size", "12px")
        .style("fill", "black")
        .style("font-weight", "bold");
  
      // Add labels to links for match level
      svg.append("g")
        .selectAll(".linkLabel")
        .data(links)
        .enter().append("text")
        .attr("class", "linkLabel")
        .attr("x", d => (d.source.x + d.target.x) / 2)
        .attr("y", d => (d.source.y + d.target.y) / 2)
        .attr("dy", -10)
        .text(d => d.value)
        .style("font-size", "10px")
        .style("fill", "black");
  
      simulation.on("tick", () => {
        link
          .attr("x1", d => d.source.x)
          .attr("y1", d => d.source.y)
          .attr("x2", d => d.target.x)
          .attr("y2", d => d.target.y);
  
        node
          .attr("cx", d => d.x)
          .attr("cy", d => d.y);
  
        // Update node labels
        svg.selectAll(".nodeLabel")
          .attr("x", d => d.x)
          .attr("y", d => d.y);
  
        // Update link labels
        svg.selectAll(".linkLabel")
          .attr("x", d => (d.source.x + d.target.x) / 2)
          .attr("y", d => (d.source.y + d.target.y) / 2);
      });
  
      function getMatchLevelClass(matchLevel) {
  if (matchLevel === 5) return "perfect";
  if (matchLevel === 4) return "good";
  if (matchLevel === 3) return "okay";
  if (matchLevel === 2) return "bad";
  if (matchLevel === 1) return "disaster"; // Added "disaster" for level 1
}

// Update the link labels to show descriptions instead of numeric values
svg.append("g")
  .selectAll(".linkLabel")
  .data(links)
  .enter().append("text")
  .attr("class", "linkLabel")
  .attr("x", d => (d.source.x + d.target.x) / 2)
  .attr("y", d => (d.source.y + d.target.y) / 2)
  .attr("dy", -10)
  .text(d => {
    switch (d.value) {
      case 1: return "disaster";
      case 2: return "bad";
      case 3: return "okay";
      case 4: return "good";
      case 5: return "perfect";
      default: return "";
    }
  }) // Updated label text to descriptions
  .style("font-size", "10px")
  .style("fill", "black");

  
  function getLinkColor(matchLevel) {
    if (matchLevel === 5) return "green";  // Perfect
    if (matchLevel === 4) return "blue";   // Good
    if (matchLevel === 3) return "yellow"; // Okay
    if (matchLevel === 2) return "orange"; // Bad
    return "red";                          // Disaster
    }
  
      function dragStarted(event) {
        if (!event.active) simulation.alphaTarget(0.3).restart();
        event.subject.fx = event.subject.x;
        event.subject.fy = event.subject.y;
      }
  
      function dragging(event) {
        event.subject.fx = event.x;
        event.subject.fy = event.y;
      }
  
      function dragEnded(event) {
        if (!event.active) simulation.alphaTarget(0);
        event.subject.fx = null;
        event.subject.fy = null;
      }
    });
  </script>
  
  <!-- <svg width="1000" height="800" style="border: 1px solid #ccc; position"></svg>   -->

  <style>
    .container {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
  
    svg {
      border: 1px solid #ccc;
    }
  </style>
  
  <div class="container">
    <svg width="1000" height="800"></svg>
  </div>