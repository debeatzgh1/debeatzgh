<!-- HTML PREVIEW EDITOR -->
<section id="html-preview-editor" class="max-w-4xl mx-auto mt-8 px-4">
  <div class="bg-white rounded-xl shadow p-5">
    <div class="flex items-center justify-between mb-4">
      <div>
        <h3 class="text-xl font-bold">HTML Preview Editor</h3>
        <p class="text-sm text-gray-600">Edit HTML/CSS/JS and preview it live. You can also load the Curly Chainsaw project directly into the preview.</p>
      </div>
      <div class="flex gap-2">
        <button id="load-project" class="px-3 py-2 rounded bg-blue-600 text-white font-semibold">Load Curly Chainsaw</button>
        <button id="fetch-into-editor" class="px-3 py-2 rounded bg-indigo-600 text-white font-semibold">Fetch into Editor</button>
      </div>
    </div>

    <div class="grid gap-4" style="grid-template-columns: 1fr;">

      <!-- Editor -->
      <div>
        <label class="block text-sm font-semibold mb-2">Editor (HTML/CSS/JS)</label>
        <textarea id="html-editor" style="width:100%; height:280px; font-family:ui-monospace, SFMono-Regular, Menlo, Monaco, 'Roboto Mono', monospace; font-size:13px; padding:12px; border-radius:10px; border:1px solid #e5e7eb;">
<!-- Start typing or press "Fetch into Editor" to load from the Curly Chainsaw project -->
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1" />
    <title>Live Preview</title>
    <style>body{font-family: Inter, ui-sans-serif, system-ui; padding: 20px;}</style>
  </head>
  <body>
    <h1>Hello — live preview!</h1>
    <p>Edit HTML/CSS/JS on the left, press Run to preview here.</p>
    <script>console.log('preview loaded')</script>
  </body>
</html>
        </textarea>

        <div class="mt-3 flex gap-2">
          <button id="run-btn" class="px-4 py-2 rounded bg-green-600 text-white font-semibold">Run</button>
          <button id="open-new" class="px-4 py-2 rounded bg-gray-200 text-gray-800">Open in New Tab</button>
          <button id="clear-editor" class="px-4 py-2 rounded bg-red-500 text-white">Clear</button>
        </div>
      </div>

      <!-- Preview -->
      <div>
        <label class="block text-sm font-semibold mb-2">Preview (iframe)</label>
        <div style="position:relative; border-radius:10px; overflow:hidden; border:1px solid #e5e7eb;">
          <iframe id="preview-frame"
                  title="HTML Preview"
                  sandbox="allow-scripts allow-forms allow-popups allow-same-origin"
                  style="width:100%; height:420px; border:0; display:block; background:white;">
          </iframe>
        </div>
        <p id="preview-note" class="text-xs text-gray-500 mt-2">Tip: if a site refuses to embed, the editor will offer to open it in a new tab.</p>
      </div>

    </div>
  </div>
</section>

<script>
/* HTML Preview Editor Script */
(function () {
  const runBtn = document.getElementById('run-btn');
  const editor = document.getElementById('html-editor');
  const iframe = document.getElementById('preview-frame');
  const openNew = document.getElementById('open-new');
  const clearBtn = document.getElementById('clear-editor');
  const loadProject = document.getElementById('load-project');
  const fetchInto = document.getElementById('fetch-into-editor');
  const targetUrl = 'https://debeatzgh1.github.io/curly-chainsaw/';

  // Utility: write to iframe using srcdoc (safe for same-origin preview)
  function writePreview(html) {
    try {
      // Use srcdoc for immediate inline preview
      iframe.srcdoc = html;
    } catch (err) {
      // Fallback: write via contentWindow (may be blocked cross-origin)
      try {
        const doc = iframe.contentWindow.document;
        doc.open();
        doc.write(html);
        doc.close();
      } catch (e) {
        console.warn('Could not write to iframe directly:', e);
      }
    }
  }

  // Run the editor content inside iframe
  runBtn.addEventListener('click', function () {
    const content = editor.value;
    writePreview(content);
  });

  // Open current editor content in a new tab as a data URL (useful for download/open)
  openNew.addEventListener('click', function () {
    const content = editor.value;
    const blob = new Blob([content], { type: 'text/html' });
    const url = URL.createObjectURL(blob);
    window.open(url, '_blank');
    // release object URL after a bit
    setTimeout(() => URL.revokeObjectURL(url), 5000);
  });

  // Clear editor
  clearBtn.addEventListener('click', function () {
    if (confirm('Clear editor content?')) editor.value = '';
    // optionally clear preview
    iframe.srcdoc = '<!doctype html><html><body style="font-family: sans-serif; padding:20px;"><em>Preview cleared</em></body></html>';
  });

  // Try to load the live project into the iframe (first attempt embed)
  loadProject.addEventListener('click', async function () {
    // Attempt to set iframe src directly and detect load failure due to X-Frame-Options
    iframe.src = targetUrl;

    // If the target denies embedding, we can't reliably detect via JS, so set a timer and then test via try/catch on access
    const timeout = setTimeout(() => {
      // Try to access iframe contentWindow to detect cross-origin/blocked
      let blocked = false;
      try {
        // Accessing location.href of cross-origin iframe throws or returns different origin
        const href = iframe.contentWindow.location.href;
        // If same origin, OK; otherwise, treat as blocked only if not matching
        if (!href || href.indexOf(window.location.origin) !== 0) {
          // But some servers allow embedding—if same origin check fails, still may be accessible; we'll attempt safe property read
          // we won't assume blocked here, allow user to view iframe
        }
      } catch (err) {
        blocked = true;
      }

      if (blocked) {
        // fallback: offer to open in new tab (cannot embed)
        if (confirm('This site prevents embedding (X-Frame-Options/CSP). Open in a new tab instead?')) {
          window.open(targetUrl, '_blank');
        }
        // reset iframe src to blank
        iframe.src = 'about:blank';
      }
    }, 1200); // wait 1.2s for load attempt

    // Also remove the timeout when load event fires successfully
    iframe.onload = function () {
      clearTimeout(timeout);
      // if it loads fine, optionally show message
      // no-op
    };
  });

  // Fetch the HTML of the project into the editor (works if CORS allows)
  fetchInto.addEventListener('click', async function () {
    try {
      const resp = await fetch(targetUrl, { mode: 'cors' });
      if (!resp.ok) throw new Error('Network response not OK: ' + resp.status);
      const html = await resp.text();
      editor.value = html;
      // Optionally run it immediately
      writePreview(html);
    } catch (err) {
      console.warn('Fetch failed (CORS or network):', err);
      if (confirm('Could not fetch project HTML (CORS). Open project in a new tab instead?')) {
        window.open(targetUrl, '_blank');
      }
    }
  });

  // Initialize preview with editor content
  writePreview(editor.value);

})();
</script>
