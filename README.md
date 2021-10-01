# RogerThat

**RogerThat** is a standalone webserver designed to receive **TradingView** webhooks (or similar) and forward them to **Hummingbot** via websockets or REST queries.

Whilst it's purpose is to bridge **TradingView** and **Hummingbot**, it can work as a gateway / bridge between any service that sends data via Webhooks and serve / route them to *multiple* **Hummingbot** instances.

##### Menu

- [Docker](#docker)
  * [Installation](#installation)
  * [Running](#running)
  * [Stopping](#stopping)
  * [Configuration](#configuration)
    + [Public access](#public-access)
- [Usage](#usage)
  * [TradingView Webhooks](#tradingview-webhooks)
  * [Hummingbot Connection](#hummingbot-connection)
  * [Test Connection](#test-connection)
- [Source Installation (ADVANCED!!!)](#source-installation-advanced)

# Docker
## Installation

[**Install Docker**](https://docs.docker.com/get-docker/)

Download and extract this [**whole repository**](https://github.com/TheHolyRoger/RogerThat/archive/refs/heads/master.zip).

<details>
<summary>Linux/Mac</summary>

```bash
wget https://github.com/TheHolyRoger/RogerThat/archive/refs/heads/master.zip
unzip master.zip
```

Change directory:
```bash
cd RogerThat-master
```

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) :warning: **You must always run scripts from the main project directory, do not switch to the `scripts` directory**

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) :warning: **Do not run as root.**

</details>
<details>
<summary>Windows</summary>

Manually download and extract the [**repository zip file**](https://github.com/TheHolyRoger/RogerThat/archive/refs/heads/master.zip).

Open up Windows CMD and **switch directory to the extracted zip folder**.

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) :warning: **You must always run scripts from the main project directory, do not switch to the `scripts` directory**

</details>

Then run the following commands to launch the docker image:

<details>
<summary>Linux/Mac</summary>

```bash
./scripts/start_docker.sh
```
</details>
<details>
<summary>Windows</summary>

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) :warning: **If using windows, make sure to run the .bat scripts using windows CMD, not "Git Bash".**

Use git bash only for `git` commands, do not run these scripts from git bash as they will not work.

```bat
scripts\start_docker.bat
```
</details>

___

## Running

<details>
<summary>Linux/Mac</summary>

```bash
./scripts/start_docker.sh
```

OR as daemon (in background):

```bash
./scripts/start_docker.sh -d
```
</details>
<details>
<summary>Windows</summary>

```bat
scripts\start_docker.bat
```

OR as daemon (in background):

```bat
scripts\start_docker.bat -d
```
</details>

Upon successful launch, navigating to `http://localhost/` in your browser (or your public domain) will show a 404 not found page.

___

## Stopping

Run the following command to stop RogerThat:
```bash
docker-compose stop
```

___

## Configuration

You can use the config setup script via docker by running the following commands:

<details>
<summary>Linux/Mac</summary>

```bash
scripts/setup_config.sh --help
```

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) :warning: **Do not run the setup script via python, always run it via `scripts/setup_config.sh`.**

</details>
<details>
<summary>Windows</summary>

```bat
scripts\setup_config.bat --help
```

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) :warning: **Do not run the setup script via python, always run it via `scripts\setup_config.bat`.**

</details>

___

### Public access

<details>
<summary>Expand ...</summary>

Since **TradingView** requires a publicly accessible URL for webhook alerts, you'll need to use your own domain name, or your public IP address.

You'll also need to open up (and forward) port **80** (or **443** if using HTTPS) in your firewall/router to the machine running **RogerThat**.

You must change/set your hostname before enabling HTTPS with **letsencrypt**

**(Do NOT open up port 10073 externally)**

#### Change Hostname

<details>
<summary>Expand ...</summary>

Change the hostname to listen on for the public **TradingView** webhook with the following commands:

(Do not use a full URL here, the hostname is the part of the URL after https:// and before any other slashes)

<details>
<summary>Linux/Mac</summary>

```bash
scripts/setup_config.sh --hostname yourhostname.com
scripts/setup_config.sh --hostname 1.2.3.4
```
</details>
<details>
<summary>Windows</summary>

```bat
scripts\setup_config.bat --hostname yourhostname.com
scripts\setup_config.bat --hostname 1.2.3.4
```
</details>

If using your own domain name, it is recommended to use a long and not obvious subdomain as the hostname eg: `thereisnotraderhere.mydomain.com`.

</details>

#### Cloudflare (Recommended)

<details>
<summary>Expand ...</summary>

It is recommended to use [Cloudflare](https://www.cloudflare.com/) to proxy and mask your IP address for public operation.

You can use services like [DNS-o-matic](https://dnsomatic.com/) with your home dynamic IP to keep it updated and proxied with [Cloudflare](https://www.cloudflare.com/).

[More information in the help article here](https://support.cloudflare.com/hc/en-us/articles/360020524512-Manage-dynamic-IPs-in-Cloudflare-DNS-programmatically#h_161458650101544484552881)

</details>

#### Dynamic Domain Names (Optional)

<details>
<summary>Expand ...</summary>

Services you can use for dynamic DNS with a non-static public IP address are:

* [DNS-O-matic](https://dnsomatic.com/) (Recommended, with Cloudflare)
* [No-IP](https://www.noip.com/)
* [Afraid](https://afraid.org/)
* [Duck DNS](https://duckdns.org/)
* [Dynu](http://www.dynu.com/)

</details>

#### Enabling HTTPS (Recommended, required for Cloudflare)

<details>
<summary>Expand ...</summary>

To setup ([LetsEncrypt](https://letsencrypt.org/getting-started/) run the following commands.

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) :warning: **You must set your hostname first and forward port 80 on your firewall!**

<details>
<summary>Linux/Mac</summary>

```bash
scripts/generate_cert_letsencrypt.sh
```

</details>
<details>
<summary>Windows</summary>

```bat
scripts/generate_cert_letsencrypt.bat
```
</details>

Or run the following command to generate a self-signed key pair:

<details>
<summary>Linux/Mac</summary>

```bash
scripts/generate_cert_self_signed.sh
```

</details>
<details>
<summary>Windows</summary>

```bat
scripts\generate_cert_self_signed.bat
```
</details>

After enabling HTTPS you can now forward port 443, close port 80 and start RogerThat.

![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) :warning: **It is recommended to close port 80**

</details>

</details>

___

# Usage

## TradingView Webhooks

Set up your **TradingView** alert URL like this:

```html
http://<public-ip-or-domain-name>/api/tv_webhook/?api_key=<tradingview-apikey>
```

Where `<public-ip-or-domain-name>` is your domain name or public IP address and `<tradingview-apikey>` is the generated api key found in `web_server.yml` under `api_allowed_keys_tv`.

### JSON Data for TradingView alerts.

Alerts must be formatted as JSON, the only required parameter is `name`.
*(this parameter key name is configurable in `configs/tradingview.yml`)*

The `name` key or one of the `tradingview_descriptor_fields` must be present in the JSON data to be accepted, but the value can be null or empty. This is transmitted to **Hummingbot** as `event_descriptor`

Using an empty value for `name` ignores **Hummingbot**'s *Remote Command Executor* routing names.

<details>
<summary>Example alert data:</summary>

Simple Start command

```json
{
    "name": "hummingbot_instance_1",
    "command": "start",
}
```

Simple Stop command

```json
{
    "name": "hummingbot_instance_1",
    "command": "stop",
}
```

Alert with all fields using Pine variables

```json
{
    "name": "hummingbot_instance_1",
    "timestamp": "{{timenow}}",
    "exchange": "{{exchange}}",
    "symbol": "{{ticker}}",
    "interval": "{{interval}}",
    "price": "{{close}}",
    "volume": "{{volume}}",
    "command": "{{strategy.market_position}}",
    "inventory": "{{strategy.order.comment}}"
}
```

</details>

___

## Hummingbot Connection

You can connect the **Hummingbot** _Remote Command Executor_ to the **RogerThat** websocket with this URL:
```html
ws://localhost:10073/wss
```

### Example Config

<details>
<summary>Example Config ...</summary>

Use something like the following config to connect **RogerThat** to **Hummingbot** via the **Remote Command Executor**.

This config is found inside your main hummingbot folder then `conf\conf_global.yml`

```yaml
# Remote commands
remote_commands_enabled: true
remote_commands_api_key: a9ba4b61-6f6d-41cf-85c3-7cfdfcbea0f3
remote_commands_ws_url: ws://localhost:10073/wss
# Specify a routing name for use with multiple Hummingbot instances
remote_commands_routing_name: hummingbot_instance_1
# Recommended to keep this on so no events are missed in the case of a network drop out.
remote_commands_ignore_first_event: true
# Whether to disable console command processing for remote command events.
# Best to disable this if using in custom scripts or strategies
remote_commands_disable_console_commands: false
# You can specify how to translate received commands to Hummingbot commands here
# eg.
# remote_commands_translate_commands:
#   long: start
#   short: stop
remote_commands_translate_commands:
  long: start
  short: stop
```

</details>


### Filtering events

<details>
<summary>Expand ...</summary>

To filter events received based on the `event_descriptor`, change your websockets URL to:
```html
ws://localhost:10073/wss/<event-descriptor>
```

Where `event-descriptor` matches the `event_descriptor` or *name* value of the events you wish to receive.

</details>


### Command Shortcuts

<details>
<summary>Expand ...</summary>

Command shortcuts can be defined in Hummingbot's `conf_global.yml`, for more information see here: https://docs.hummingbot.io/operation/config-files/#create-command-shortcuts

</details>

___

## Test Connection

<details>
<summary>Test Connection ...</summary>

You can enable and disable websockets authentication in the config with these commands.
(You must disable websockets authentication for the in-browser test to work.)

<details>
<summary>Linux/Mac</summary>

```bash
scripts/setup_config.sh --enable-websocket-auth
scripts/setup_config.sh --disable-websocket-auth
```
</details>
<details>
<summary>Windows</summary>

```bat
scripts\setup_config.bat --enable-websocket-auth
scripts\setup_config.bat --disable-websocket-auth
```
</details>

Test the websocket feed in your browser with this js code:

```javascript
var ws = new WebSocket('ws://localhost:10073/wss');
ws.onmessage = function (event) {
    console.log(event.data);
};
```

Or run the python test listener (requires source installation steps but can authenticate):

```bash
python tests/test_websocket.py
```

Or test the REST url here:

```html
http://localhost:10073/api/hbot/?api_key=<hummingbot-apikey>
```

Where `<hummingbot-apikey>` is the generated api key found in `web_server.yml` under `api_allowed_keys_hbot`.

</details>

___

# Source Installation (ADVANCED!!!)

<details>
<summary>Steps to install from **source** instead of docker ...</summary>

## Installation

[Install Miniconda](https://docs.conda.io/en/latest/miniconda.html) (or Anaconda)

Clone this repository.

```bash
git clone git@github.com:TheHolyRoger/RogerThat.git
```

Change directory:
```bash
cd RogerThat
```

Set up and activate the environment with the following command.

<details>
<summary>Linux/Mac</summary>

```bash
./scripts/update_environment.sh
```
</details>
<details>
<summary>Windows</summary>

```bat
scripts\update_environment.bat
```
</details>

Run the following command to generate the default configs:
```bash
scripts/setup.py -s
```

Edit the configs in `./configs` or via the `setup.py` command.

___

## Running

From source:

```bash
bin/start_rogerthat.py
```
</details>

___

# License
[MIT](https://choosealicense.com/licenses/mit/)