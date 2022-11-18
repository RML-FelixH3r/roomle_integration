<script>
  import RoomleConfiguratorApi from "@roomle/embedding-lib/roomle-configurator-api";
  import {onMount} from "svelte";
  let configuratorContainer;
  onMount(async () => {
    const itemId = 'wanzl:salsa_cart';
    const configuratorId = 'wanzl';
    const catalogRootTag='moc_sample_catalog';
    const options = {
      skin: {
        // 'primary-color': 'blue',
        // 'color-on-primary': 'red',
        // 'cta-color': 'green',
        // 'color-on-cta': 'yellow',
      },
    }
    const configurator = await RoomleConfiguratorApi.createConfigurator(configuratorId, configuratorContainer, options);
    console.log(configuratorContainer.firstElementChild);
    configuratorContainer.firstElementChild.style.border = "none";

    // load wanted object
    await configurator.ui.loadObject(itemId);

    // init sample callback (listen to sdk events)
    configurator.ui.callbacks.onPartListUpdate = (partList, hash) => console.log('partlist and hash: ', partList, hash);

    // get params of scene object
    const params = await configurator.extended.getParametersOfPlanObject();
    await configurator.extended.showDimensions();
    console.log('parameter: ', params);

    // listen on button click and change param with Typescript
    configurator.ui.callbacks.onButtonClicked = async (name) => {
      if(name === 'savedraft'){
        await configurator.extended.setParameter(params[1], params[1].validValues[2].value);
      }
    }
  });

</script>

<main>
  <h1>Roomle integration</h1>
  <div bind:this={configuratorContainer} id="configurator-container"></div>
</main>

<style>
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }
  #configurator-container {
    width: 100%;
    height: 90vh;
    border: none;
  }
</style>
