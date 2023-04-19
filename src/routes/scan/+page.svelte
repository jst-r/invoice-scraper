<script lang="ts">
  import QrScanner from "qr-scanner";
  import { onDestroy, onMount } from "svelte";

  let videoElem: HTMLVideoElement;

  let qrScanner: QrScanner;

  let seen = new Set<string>();

  onMount(() => {
    qrScanner = new QrScanner(
      videoElem,
      (result) => {
        let uri = result.data;
        if (seen.has(uri)) {
          return;
        }
        console.log(uri);
        seen.add(uri);
      },
      {
        highlightScanRegion: true,
        highlightCodeOutline: true,
        returnDetailedScanResult: true,
      }
    );

    qrScanner.setCamera("environment");
    qrScanner.start();
  });

  onDestroy(() => {
    qrScanner.destroy();
    qrScanner = undefined as any;
  });

  //https://mapr.tax.gov.me/ic/#/verify?iic=01ea0b89937c6f70e12629def76150b0&tin=02047403&crtd=2023-01-25T09:23:44+01:00&ord=6853&bu=mi994dc812&cr=zn383va091&sw=uu980jl537&prc=76.27

  QrScanner.hasCamera().then(console.log);
</script>

<a href="..">Back</a>

<div>Scanner</div>

<!-- svelte-ignore a11y-media-has-caption -->
<video bind:this={videoElem} />

{#each [...seen.values()] as uri}
  <div>
    {uri}
  </div>
{/each}
