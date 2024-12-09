<script>
  import LiveCard from './LiveCard.svelte';
  import Chart from './Chart.svelte';
  import LocationSelector from './LocationSelector.svelte';
  import Tips from './Tips.svelte';
  import History from './History.svelte';
  import Notification from './Notification.svelte';
  import StatisticsCounter from './StatisticsCounter.svelte';
  import WaterTrends from './WaterTrends.svelte';

  let waterData = null;
  let history = [];
  let selectedLocation = "New York";
  let graphData = [];
  let labels = [];
  let totalChecks = 1200;
  let safeLocations = 890;
  let avgPH = 7.4;

  // Simulate fetching water quality data based on location
  async function fetchWaterData(location) {
    try {
      const response = await new Promise((resolve) =>
        setTimeout(() => {
          resolve({
            ph: Math.random() * (9 - 6) + 6,
            pollutants: Math.floor(Math.random() * 100),
            status: Math.random() > 0.5 ? "Safe" : "Unsafe",
            graph: {
              labels: ["Jan", "Feb", "Mar", "Apr"],
              data: [
                Math.random() * (9 - 6) + 6,
                Math.random() * (9 - 6) + 6,
                Math.random() * (9 - 6) + 6,
                Math.random() * (9 - 6) + 6,
              ],
            },
          });
        }, 1000)
      );
      return response;
    } catch (error) {
      console.error("Error fetching water data:", error);
    }
  }

  async function updateData() {
    if (!selectedLocation) return;

    const data = await fetchWaterData(selectedLocation);
    if (data) {
      waterData = data;
      graphData = data.graph.data;
      labels = data.graph.labels;

      // Add to history if not already present
      const alreadyInHistory = history.some(
        (item) =>
          item.location === selectedLocation &&
          item.ph === data.ph &&
          item.pollutants === data.pollutants
      );

      if (!alreadyInHistory) {
        history = [
          ...history,
          {
            location: selectedLocation,
            ph: data.ph,
            pollutants: data.pollutants,
            status: data.status,
          },
        ];
      }
    }
  }

  // Trigger update when location changes
  $: if (selectedLocation) updateData();
</script>

<div class="dashboard">
  <!-- Location Selector -->
  <LocationSelector bind:selectedLocation />

  <!-- Notification -->
  {#if waterData}
    <Notification status={waterData.status} />
  {/if}

  <!-- Statistics Counters -->
  <div class="stats">
    <StatisticsCounter value={totalChecks} />
    <StatisticsCounter value={safeLocations} />
    <StatisticsCounter value={avgPH * 100} />
  </div>

  <!-- Metrics Section -->
  {#if waterData}
    <div class="metrics">
      <LiveCard title="pH Level" value={waterData.ph.toFixed(1)} unit="" color="#28a745" />
      <LiveCard title="Pollutants" value={waterData.pollutants} unit="ppm" color="#ffc107" />
      <LiveCard title="Status" value={waterData.status} unit="" color="#17a2b8" />
    </div>
    <Chart labels={labels} data={graphData} label="pH Levels Over Time" />
    <Tips status={waterData.status} />
  {/if}

  <!-- Water Trends -->
  <WaterTrends />

  <!-- History Section -->
  <History history={history} />
</div>

<style>
  .dashboard {
    display: flex;
    flex-direction: column;
    gap: 20px;
    padding: 20px;
    background-color: var(--bg-color);
    border-radius: 8px;
  }

  .stats {
    display: flex;
    justify-content: space-around;
    margin: 20px 0;
  }

  .metrics {
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;
    margin-bottom: 20px;
  }

  @media (max-width: 768px) {
    .metrics,
    .stats {
      flex-direction: column;
      align-items: center;
    }
  }
</style>
