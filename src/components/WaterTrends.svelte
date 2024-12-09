<script>
    import { onMount } from "svelte";
  
    let trends = [
      { month: "January", averagePH: 7.4, safePercentage: 90 },
      { month: "February", averagePH: 7.2, safePercentage: 85 },
      { month: "March", averagePH: 7.5, safePercentage: 88 },
    ];
  </script>
  
  <div class="water-trends">
    <h2>Water Quality Trends</h2>
    <table>
      <thead>
        <tr>
          <th>Month</th>
          <th>Average pH</th>
          <th>Safe Percentage</th>
        </tr>
      </thead>
      <tbody>
        {#each trends as trend}
          <tr>
            <td>{trend.month}</td>
            <td>{trend.averagePH}</td>
            <td>{trend.safePercentage}%</td>
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  
  <style>
    .water-trends {
      margin-top: 20px;
      background-color: var(--card-bg);
      padding: 20px;
      border-radius: 8px;
      border: 1px solid var(--card-border);
    }
  
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
    }
  
    th, td {
      border: 1px solid var(--card-border);
      padding: 10px;
      text-align: center;
    }
  
    th {
      background-color: var(--primary-color);
      color: white;
    }
  </style>
  