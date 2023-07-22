<script lang="ts">
  import type monaco from 'monaco-editor';
  import { onMount } from 'svelte';
  import editorWorker from 'monaco-editor/esm/vs/editor/editor.worker?worker';
  import jsonWorker from 'monaco-editor/esm/vs/language/json/json.worker?worker';
  import tsWorker from 'monaco-editor/esm/vs/language/typescript/ts.worker?worker';

  let divEl: HTMLDivElement = null;
  let editor: monaco.editor.IStandaloneCodeEditor;
  let Monaco;

  onMount(async () => {
      // @ts-ignore
      self.MonacoEnvironment = {
          getWorker: function (_moduleId: any, label: string) {
              if (label === 'json') {
                  return new jsonWorker();
              }
              // if (label === 'css' || label === 'scss' || label === 'less') {
              //     return new cssWorker();
              // }
              // if (label === 'html' || label === 'handlebars' || label === 'razor') {
              //     return new htmlWorker();
              // }
              if (label === 'typescript' || label === 'javascript') {
                  return new tsWorker();
              }
              return new editorWorker();
          }
      };

      Monaco = await import('monaco-editor');
      editor = Monaco.editor.create(divEl, {
          value: ['function x() {', '\tconsole.log("Hello world!");', '}'].join('\n'),
          language: 'json',
          minimap: { enabled: false },
            scrollBeyondLastLine: false
      });

      return () => {
          editor.dispose();
      };
  });
</script>

<div class="h-screen-parent">
<div bind:this={divEl} class="h-screen" />
</div>

<style>
  .h-screen {
    height: 80%;
    position:relative;
    display:block
  }
.h-screen-parent{
  position:absolute;
  top: 100px;
  left:0px;
  right:0px;
  height:100%;
  display:block;
  text-align: initial;
}
</style>