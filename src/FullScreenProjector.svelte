<script>
  import { onMount } from 'svelte'
  import { sendCommand } from './obs.js'
  import SourceButton from './SourceButton.svelte'

  let monitors = [];
  let monitorIndex = 0;

  onMount(async function () {
    let data = await sendCommand('GetMonitorList')
    monitors = data.monitors || []
    console.log('GetMonitorList', monitors)
  })

  function buttonClicker () {
    return async function () {
      await sendCommand('OpenVideoMixProjector', { videoMixType: "OBS_WEBSOCKET_VIDEO_MIX_TYPE_PROGRAM", monitorIndex })
    }
  }
</script>

<div>
  FullScreenProjector
  <select bind:value={monitorIndex} title="Select fullScreenProjector destination">
    {#each monitors as monitor}
      <option value={monitor.monitorIndex}>{monitor.monitorName}</option>
    {/each}
  </select>
  <SourceButton
    name="FullScreenProjector"
    on:click={buttonClicker()}
  />
</div>

<style>
  div {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: .5rem;
    margin-bottom: 2rem;
  }
</style>
