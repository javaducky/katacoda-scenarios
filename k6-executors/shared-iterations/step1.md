The Katacoda `editor-terminal` UI Layout combines an Editor with a Terminal.
When changes are made to files using the editor, they are automatically sync'ed
to the user's home directory.

# Index.json

The editor will display and interact with files relative to the `uieditorpath`
you set in the `index.json` file for your scenario.

<pre>
"environment": {
  "uilayout": "editor-terminal",
  "uieditorpath": "/root/scripts"
}
</pre>

If not specified, the `uieditorpath` will default to the `/root` directory.

You can pre-open a set of files in the editor by defining a `files` array in the root
of the `index.json` file.

<pre>
"files": [
    "test.js"
]
</pre>

Files open in the editor window are updated as you make changes in the editor.
If the files do not already exist, the tabs will still open for them, but they
will not be created until you have updated the content in the editor window.

You can include a path with these filenames. Paths should be given
relative to the `uieditorpath`. Only files under that path will be
diplayed in the editor's directory tree.

The syntax highlighting for the editor is automatically inferred from the file
extension. This can be enforced by adding a `uisettings` to your environment.

<pre>
"environment": {
    "uilayout": "editor-terminal",
    "uieditorpath": "/root/scripts",
    "uisettings": "javascript"
},
</pre>

# Helper Functionality

You can replace, prepend, append, and insert text into an open editor file.

Copy file to editor:

<pre class="file" data-filename="test.js" data-target="replace">import http from 'k6/http';
import { sleep } from 'k6';

export const options = {
  scenarios: {
    contacts: {
      executor: 'shared-iterations',
      vus: 10,
      iterations: 200,
      maxDuration: '30s',
    },
  },
};

export default function () {
  http.get('https://test.k6.io/contacts.php');
  sleep(0.5);
}
</pre>

The following snippet will prepend the contents of the editor:

<pre class="file" data-filename="test.js" data-target="prepend">console.log("Starting...")
</pre>

Within the Markdown, include:

<pre>
&#x3C;pre class=&#x22;file&#x22; data-filename=&#x22;test.js&#x22; data-target=&#x22;prepend&#x22;&#x3E;console.log(&#x22;Starting...&#x22;)
&#x3C;/pre&#x3E;
</pre>

The following snippet will append the contents of the editor:

<pre class="file" data-filename="test.js" data-target="append">console.log("Finishing...")
</pre>

Within the Markdown, include:

<pre>
&#x3C;pre class=&#x22;file&#x22; data-filename=&#x22;test.js&#x22; data-target=&#x22;append&#x22;&#x3E;console.log(&#x22;Finishing...&#x22;)
&#x3C;/pre&#x3E;
</pre>

Using the `data-target="insert"` attribute you can instruct the editor to insert the text in a particular position marked by `data-marker` attribute.

<pre class="file" data-filename="test.js" data-target="append">#TODO-insert
</pre>

<pre>
&#x3C;pre class=&#x22;file&#x22; data-filename=&#x22;test.js&#x22; data-target=&#x22;insert&#x22; data-marker=&#x22;#TODO-insert&#x22;&#x3E;
console.log(&#x22;Inserted value using the data-marker attribute...&#x22;)
&#x3C;/pre&#x3E;
</pre>

<pre class="file" data-filename="test.js" data-target="insert" data-marker="#TODO-insert">
console.log("Inserted value using the data-marker attribute...")
</pre>

## New Files

Added files in the `uieditorpath` will automatically be added to the tree in the
editor. A markdown extension also makes it easy to open new files in the editor.

Create a new file via the CLI:
`touch newFile.js`{{execute}}

The Markdown is:
<pre>`touch newFile.js`{{execute}}</pre>

This can then be opened:
`newFile.js`{{open}}

The Markdown is:
<pre>`newFile.js`{{open}}</pre>
