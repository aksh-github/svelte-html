<script lang="ts">
  let selectDiv = null;

  function selectText() {
    // const selectable = document.getElementById("selectable");

    if (document?.selection) {
      console.log("click");
      const range = document.body?.createTextRange();
      range.moveToElementText(selectDiv);
      range.select();
    } else if (window.getSelection) {
      console.log("click else");
      const range = document.createRange();
      range.selectNode(selectDiv);
      window.getSelection().removeAllRanges();
      window.getSelection().addRange(range);
    }
  }

  function copyText(e) {
    const r = document.createRange();
    r.selectNode(selectDiv);
    window.getSelection().removeAllRanges();
    window.getSelection().addRange(r);
    document.execCommand("copy");
    window.getSelection().removeAllRanges();
    e.target.innerText = "Copied";
    const id = setTimeout(() => {
      clearTimeout(id);
      e.target.innerText = "Copy";
    }, 2000);
  }
</script>

<div bind:this={selectDiv}>http://example.com/page.htm</div>
<button on:click={selectText}>select</button>
<button on:click={copyText}>Copy</button>
