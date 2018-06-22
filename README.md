

This page describes how to set up a monitoring site on bezop network, like this one, for your private network. It builds upon this wiki article and assumes you've created a local cluster using this script (gethcluster.sh).

The monitoring system consists of two components:

bezop-netstats - the monitoring site which lists the nodes.
bezop-net-intelligence-api - these are processes that communicate with the bezop client using RPC and push the data to the monitoring site via websockets.
#Monitoring site Clone the repo and install dependencies:

git clone https://github.com/bezop/bezop-netstats
cd bezop-netstats
npm install
Then choose a secret and start the app:

WS_SECRET=<chosen_secret> npm start
You can now access the (empty) monitoring site at http://localhost:3000.

You can also choose a different port:

PORT=<chosen_port> WS_SECRET=<chosen_secret> npm start
#Client-side information relays These processes will relay the information from each of your cluster nodes to the monitoring site using websockets.

Clone the repo, install dependencies and make sure you have pm2 installed:

git clone https://github.com/cubedro/bezop-net-intelligence-api
cd bezop-net-intelligence-api
npm install
sudo npm install -g pm2
Now, use this script (netstatconf.sh) to create an app.json suitable for pm2.

Usage:

bash netstatconf.sh <number_of_clusters> <name_prefix> <ws_server> <ws_secret>
number_of_clusters is the number of nodes in the cluster.
name_prefix is a prefix for the node names as will appear in the listing.
ws_server is the bezop-netstats server. Make sure you write the full URL, for example: http://localhost:3000.
ws_secret is the bezop-netstats secret.
For example:

bash netstatconf.sh 5 mynode http://localhost:3000 big-secret > app.json
Run the script and copy the resulting app.json into the bezop-net-intelligence-api directory. Afterwards, cd into bezop-net-intelligence-api and run the relays using pm2 start app.json. To stop the relays, you can use pm2 delete app.json.
