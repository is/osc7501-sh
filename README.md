# osc7501

A small command-line utility for generating OSC 7501 (Program Status Protocol) messages. Use it to report what a program is doing to a terminal that supports OSC 7501.

## Installation

```sh
mkdir -p ~/bin
curl -fsSL https://raw.githubusercontent.com/is/osc7501-sh/main/osc7501 -o ~/bin/osc7501
chmod +x ~/bin/osc7501
```

Make sure `~/bin` is in your `PATH`.

## Usage

```text
osc7501 STATE [OPTIONS]
```

Supported states: `idle`, `working`, `done`, `blocked`, `error`, and `clear`.

| Option | Description |
| --- | --- |
| `--id ID` | Record identifier. Use `/` for a hierarchy, such as `build/test`. |
| `--kind KIND` | Block reason: `permission`, `question`, or `auth`. Only valid with `blocked`. |
| `--app APP` | Application name. |
| `--title TITLE` | Short status title. |
| `--msg MESSAGE` | Status message. The script encodes it as Base64. |
| `--bel` | Terminate with BEL (`0x07`); ST (`ESC \\`) is the default. |
| `--printable` | Display control characters as `ESC`/`BEL` text for debugging. |

## Examples

Report a running task:

```sh
osc7501 working --app deploy --msg 'Deploying to production'
```

Report that approval is needed:

```sh
osc7501 blocked --kind permission --app deploy --title 'Approval required' --msg 'Continue deployment?'
```

Report completion or clear the status:

```sh
osc7501 done --app deploy --msg 'Deployment complete'
osc7501 clear
```

By default, the command writes a terminal control sequence directly to standard output. To inspect a readable representation while debugging:

```sh
osc7501 working --msg 'Processing' --printable
```

The message format is `ESC ] 7501 ; key=value[:key=value...] ST`. The `state` field is required, and `msg` is Base64-encoded. The terminal must support OSC 7501 to display the status.

## License

No license has been specified yet.

中文说明见 [README-cn.md](README-cn.md).
