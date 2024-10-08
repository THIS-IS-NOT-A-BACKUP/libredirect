<script>
  const browser = window.browser || window.chrome

  import General from "./General/General.svelte"
  import url from "./url"
  import utils from "../../assets/javascripts/utils.js"
  import { onDestroy } from "svelte"
  import servicesHelper from "../../assets/javascripts/services.js"
  import { onMount } from "svelte"
  import Sidebar from "./Sidebar.svelte"
  import { options, config } from "./stores"
  import Services from "./Services/Services.svelte"

  let _options
  const unsubscribeOptions = options.subscribe(val => {
    if (val) {
      _options = val
      browser.storage.local.set({ options: val })
    }
  })

  let _config
  const unsubscribeConfig = config.subscribe(val => (_config = val))

  onDestroy(() => {
    unsubscribeOptions()
    unsubscribeConfig()
  })

  onMount(async () => {
    let opts = await utils.getOptions()
    if (!opts) {
      await servicesHelper.initDefaults()
      opts = await utils.getOptions()
    }
    options.set(opts)
    config.set(await utils.getConfig())
  })

  let style
  $: if (_options) style = utils.style(_options, window)

  const dir = ["ar", "iw", "ku", "fa", "ur"].includes(browser.i18n.getUILanguage()) ? "rtl" : "ltr"
  document.body.dir = dir
</script>

{#if _options && _config}
  <div class={dir} {dir} {style}>
    <Sidebar />
    {#if !$url.hash || $url.hash == "#general"}
      <General />
    {:else if $url.hash.startsWith("#services")}
      <Services />
    {/if}
  </div>
{:else}
  <p>Loading...</p>
{/if}

<style>
  :global(body) {
    width: 100vw;
    height: 100vh;
    margin: 0;
    padding: 0;
  }

  div {
    height: 100%;
    display: grid;
    grid-template-columns: min-content 800px;
    margin: 0;
    padding-top: 50px;
    justify-content: center;
    font-family: "Inter", sans-serif;
    box-sizing: border-box;
    font-size: 16px;
    background-color: var(--bg-main);
    color: var(--text);
    overflow: scroll;
  }

  @media (max-width: 1250px) {
    div {
      grid-template-columns: auto;
      grid-template-rows: min-content auto;
      padding-left: 5vw;
      padding-right: 5vw;
    }
  }

  @media (max-width: 715px) {
    div {
      font-size: 14px;
      grid-template-columns: auto;
      grid-template-rows: min-content auto;
      padding-left: 5vw;
      padding-right: 5vw;
    }
  }
</style>
