Shift Network Reporter
============
This is the backend service which runs along with Shift and tracks the network status, fetches information through api and connects through WebSockets to [Shift Network Stats](https://shiftstats.miners-zone.net) to feed information.

Nodes which are offline longer then 48 hours will be automatically removed from web pages.

Shift Network Stats page does not represent the entire state of the Shift network - listing a node on this page is a voluntary process.

## Prerequisite
* Shift Node >= 6.8.5 up and running 
* nodejs >= 6 LTS
* npm

## Installation
<pre>git clone https://github.com/ExEric3/shift-reporter/
cd shift-reporter
npm install
sudo npm install -g pm2
</pre>

## Configuration
<pre>nano app.json</pre>
And modify

<pre>
[
  {
    "name"              : "shift-reporter",
    "script"            : "app.js",
    "log_date_format"   : "YYYY-MM-DD HH:mm Z",
    "merge_logs"        : false,
    "watch"             : false,
    "max_restarts"      : 10,
    "exec_interpreter"  : "node",
    "exec_mode"         : "fork_mode",
    "env":
    {
      "NODE_ENV"        : "production",
      "RPC_HOST"        : "localhost", <-  you can run Shift Reporter from different host like running Shift Node (but you will need allow IP address  access list in API and Forging access section)
      "RPC_PORT"        : "9305", <- 9305 for mainnet, 9405 testnet
      "LISTENING_PORT"  : "9305", <- 9305 for mainnet, 9405 testnet
      "INSTANCE_NAME"   : "PICK_INSTANCE_NAME", <- pick your name for example: ExEric3's Main Node
      "CONTACT_DETAILS" : "", <- contact details, email or nick on Shift Discord Server to contact in case any failure
      "NETWORK_MODE"    : "main", <- main = main network, test = test network
      "WS_SERVER"       : "https://shiftstats.miners-zone.net",
      "WS_SECRET"       : "Go to https://www.shiftproject.com/discord and pm me @ExEric3",
      "VERBOSITY"       : 0 <- 0 = nothing 1 = errors and warns 2 = logs all
    }
  }
]
</pre>

## Run
Starting
<pre>
pm2 start app.json
</pre>

Checking logs
<pre>
pm2 logs shift-reporter
</pre>

Stopping
<pre>
pm2 stop shift-reporter
</pre>

## To do
- [ ] Server SSL certificates need reload server config files every 90 days so some Shift Reportes must be restarted.
- [X] Implement reasonable check if node is forging.
- [X] Implement Email notifications for Offline nodes - this request was canceled due on Discord Server is Bot for this already.

## Credits
Thanks to [cuberdo](https://github.com/cubedro/) and his [eth-net-intelligence-api](https://github.com/cubedro/eth-net-intelligence-api). This software has been created on the top of his work.
