<script>
  import { onMount, afterUpdate } from "svelte";
  import pdfjs from "pdfjs-dist";
  import Lazy from "./Lazy.svelte";
  import Page from "./Page.svelte";

  //URL of the Document to Display
  export let url = "assets/test.pdf";
  let scale = 1;

  let documentProxy = null;
  let pageCount = 1;
  let pages = [];

  onMount(() => {
    documentProxy = pdfjs.getDocument(url);
    documentProxy.promise.then(function(D) {
      pageCount = D.numPages;
    });
  });

  afterUpdate(() => (pages = Array.from(Array(pageCount), (_, i) => i + 1)));
</script>

<style>
  .not-displayed {
    display: none;
  }

  .pdf-container {
    position: fixed;
    top: 0px;
    left: 0px;
    height: 100vh;
    overflow: scroll;
  }
</style>

<div class="pdf-container">

  {#each pages as pageNum}
    <Lazy height={1263} offset={10} fadeOption={{ delay: 110, duration: 400 }}>
      <Page {documentProxy} {pageNum} {scale} />
    </Lazy>
    <p class="not-displayed">{pageCount}</p>
  {/each}

</div>
