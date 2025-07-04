# q Professional

## Features

This extension provides q and k language support:

- k syntaxes
- q syntaxes
- q notebook(\*.qnb)
- processes group by tags
- process explorer
- query mode
  - grid powered by [ag-grid-community](https://www.ag-grid.com/) & [plotly](https://plotly.com/javascript/)
  - visualization powered by [perspective](https://perspective.finos.org/)
- language server powered by [tree-sitter](https://tree-sitter.github.io/tree-sitter/)
  - **linter**
  - **formatter**
  - rename symbol (F2)
  - go to definition (F12)
  - go to reference (Shift+F12)
  - workspace symbol (Ctrl+T)
  - document highlight
  - document symbol (Ctrl+Shift+O)
  - completion
    - identifiers defined in code
    - identifiers defined on kdb+ process
    - column names define on kdb+ process
  - completion resolve
  - signature help
  - semantic highlights
  - call hierarchy

See the [change log](https://github.com/jshinonome/vscode-k-pro/blob/master/CHANGELOG.md).

## Configuration

- To configure globally, type <kbd>ctrl</kbd>+<kbd>,</kbd> to open Settings, filter the configuration by `q-pro` and change the following values.
- To configure for workspace, type <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd>, call `Preferences: Open Workspace Settings`, filter the configuration by `q-pro` and change the following values.

| configuration                             | type    | default value                       | description                                       |
| ----------------------------------------- | ------- | ----------------------------------- | ------------------------------------------------- |
| k-lang-server.sourceFiles.globsPattern    | array   | `["**/src/**/*.q","**/src/**/*.k"]` | source folder to be included                      |
| k-lang-server.sourceFiles.ignorePattern   | array   | `["**/build","**/node_modules"]`    | folder to be excluded                             |
| k-processes.cfg.queryMode                 | string  | `Console`                           | query mode, Console, Grid or Visualization        |
| k-processes.cfg.queryGridDecimals         | number  | `3`                                 | decimals in Grid mode                             |
| k-discovery-server.cfg.refreshInterval    | number  | `30`                                | Discovery Server Refresh Interval(min)            |
| k-terminal.cfg.qBinary                    | string  | `q`                                 | q executable file or full path                    |
| k-terminal.cfg.envPath                    | string  | `''`                                | environment file relative or absolute path        |
| k-process-explorer.cfg.prevQueryLimit     | string  | `5`                                 | k process explorer preview query limit            |
| k-process-explorer.cfg.autoRefresh        | boolean | `false`                             | k process explorer auto refresh                   |
| k-process-explorer.cfg.excludedNamespaces | array   | `["q","Q","j","o","h"]`             | namespaced to be excluded from k process explorer |
| k-output.cfg.autoClear                    | boolean | `false`                             | auto clear output                                 |
| k-output.cfg.ignoreNullReturn             | boolean | `false`                             | ignore null return when using non-console mode    |
| k-output.cfg.includeQuery                 | boolean | `false`                             | include query in output                           |
| k-output.cfg.consoleSize                  | string  | `'50 160'`                          | console size for output                           |

## k-pro Language Server

The offline language server will analyze q and k source files in all 'src' folder. To increase precision, please insert ';' to indicated end of statement if necessary.

### linter and formatter

- `// k-pro-ignore-linter`: ignore linter for the following block of code
- `// k-pro-ignore-formatter`: ignore formatter for the following block of code

## q Notebook

Files with postfix \*.qnb are using notebook feature. There are 2 output mode for q notebook, switch to following query mode for different output format

- Console -> notebook console
- Grid -> notebook html
- Visualization -> notebook html

### Chart

Enable Grid or Visualization mode, insert `// @chart lines` to a q cell as the first comment and execute.

Limitations:

- use the first non-number column as label
- support up to 9 series in line/bar/histogram chart
- support up to 4 pair of series in scatter chart

> see examples here, [chart.qnb](examples/chart.qnb)

## Panel

### Processes

List processes, click to switch process. Generate tree structure from tags.

Special tag color:

- green: dev, development
- blue: uat
- red: prd, prod.

### Discovery Server

The url should be a REST API endpoint, which returns a list of `{host:string, port:number, label:string}`. The returned list will be added to Processes panel, but it won't be saved on disk.

### Process Explorer

List variables defined on the active server.

### Query History

Record query histories.

## Query Mode

Type <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd> and call `q-pro: Switch Query Mode` to switch Query Console.

### Visualization

Visualization, powered by [perspective](https://perspective.finos.org/), can pivot and virtualize table data. In Visualization mode, only table will be showed in a webview, but other result will still be in output. It will limit to 10000 rows when query a table, click the **flame** in **PROCESSES** panel , or call `q-pro: Toggle Unlimited Query`, to remove 10000 rows limit. Be noted that, Visualization only supports millisecond precision.

### Grid

Grid, powered by [ag-grid-community](https://www.ag-grid.com/) and [plotly](https://plotly.com/javascript/), can filter and sort table data. In Grid mode, only table will be showed in a webview, but other result will still be in output. It will limit to 10000 rows when query a table, click the **flame** in **PROCESSES** panel , or call `q-pro: Toggle Unlimited Query`, to remove 10000 rows limit. Grid supports nanosecond precision.

### Console(default)

Output console to an output channel. Configure `Console Size for Output` to change console size for table type output. For non-table type console size, use `system "c rows columns"` to change console size.

## Commands

### Connect to Process

Type <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd> and call `q-pro: Connect to Process` to connect to a process.

## Shortcuts

- <kbd>ctrl</kbd>+<kbd>q</kbd>: query current line
- <kbd>ctrl</kbd>+<kbd>r</kbd>: query selection
- <kbd>ctrl</kbd>+<kbd>e</kbd>: query block
- <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>q</kbd>: send current line to terminal
- <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>r</kbd>: send selection to terminal
- <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>e</kbd>: send block to terminal

To change shortcuts

1. type <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd>
2. input "shortcut"
3. open the Keyboard Shortcuts
4. search for "k-pro".

## Tips

### Enable Auto Scrolling for Output Channel

1. Type <kdb>ctrl</kdb>+<kdb>comma(,)</kdb>, open Settings, disable `Output>Smart Scroll`.
2. Turn on Auto Scrolling by clicking a small locker icon on the right top of output channel.

### Disable Word Wrap in k-pro Console of Output

Type <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd>, call `Open Setting(Json)`, and add following configuration.

```
    "[Log]": {
        "editor.wordWrap": "off"
    }
```

### No Color in k-pro Output

There may be a conflict with other extensions. Disable or uninstall them and try again.

### Enable Highlight for Attention And TODO of comments

Type <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd>, call `Open Setting(Json)`, and add following configuration.

```
    "editor.tokenColorCustomizations": {
        "textMateRules": [
            {
                "scope":"comment.line.attention",
                "settings": {
                    "fontStyle": "italic",
                    "foreground": "#B71C1C"
                }
            },
            {
                "scope":"comment.line.todo",
                "settings": {
                    "fontStyle": "italic",
                    "foreground": "#2E7D32"
                }
            }
        ]
    }
```

### Special Comment

Querying comment line `/<=> quant,prod,local-1800` will connect to `quant,prod,local-1800`.
Type <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd>, call `q-pro: Insert Active Connection Label` to insert active connection label.
