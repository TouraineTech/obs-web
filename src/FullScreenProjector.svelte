<script>
  import { onMount } from 'svelte'
  import { sendCommand } from './obs.js'
  import SourceButton from './SourceButton.svelte'

  onMount(async function () {
    const data = await sendCommand('GetMonitorList')
    // SourceFilterListReindexed ==> sourceName
    // SceneItemCreated ==> sourceUuid
    // GetSceneItemSource ==> sourceName, sourceUuid
    // GetMonitorList monitorIndex


    console.log('GetMonitorList', data)
  })
  function sceneClicker (scene) {
    return async function () {
      await sendCommand('OpenVideoMixProjector', { videoMixType: "OBS_WEBSOCKET_VIDEO_MIX_TYPE_PROGRAM", monitorIndex: 1 })
    }
  }
</script>

<ol>
  FullScreenProjector
  <SourceButton name="FullScreenProjector"
                on:click={sceneClicker()}/>
</ol>

<style>
  ol {
    list-style: None;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    gap: .5rem;
    margin-bottom: 2rem;
  }
  ol.column {
    flex-direction: column;
    gap: 1rem;
  }
  li {
    display: inline-block;
    min-width: 10rem;
    flex-grow: 1;
  }
  ol.with-icon {
    justify-content: center;
  }
  ol.with-icon li {
    min-width: 0;
    flex-grow: 0;
    flex-shrink: 1;
  }
</style>
