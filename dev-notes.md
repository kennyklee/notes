# Dev Notes

## Design Resources

* [SVG Icons - https://thenounproject.com/](https://thenounproject.com/)
* [SVG Workflow - http://www.grunticon.com/](http://www.grunticon.com/)
* [SVG - Covert SVG to chunks of code](http://www.grumpicon.com/)
* [SVG - Downloads](http://www.vecteezy.com)
* [Creative Droplets](http://creativedroplets.com/export-svg-for-the-web-with-illustrator-cc)

* [Flat icons](https://css-tricks.com/flat-icons-icon-fonts/)

## HTML5

* [http://html5hub.com/](http://html5hub.com)

## Tools

* [Run Marked2 from the Command Line](http://jblevins.org/log/marked-2-command)

## Brew, Nvm, Node

[Install NVM](http://lexsheehan.blogspot.com/2015/04/cleanly-install-nvm-node-and-npm.html)

## Dev Tool Tips

### Tips video
[JS guy video - Chrom dev console](http://www.thatjsdude.com/jsConcepts/concepts/console.html)

### In code
* Insert 'debugger;' into your code.

### Elements tab
* Right-click on element, then "Scroll into View"
* Select element, and toggle with 'H'
* HTML - DOM Break Point
  * removes node
  * changes node
  * or anything under the node.
* Shift-click color to change color format.

### Network tab

* Disable cache - so that cache cannot be saved
* Shift mouseover. Red are the files that are called.  Green are the files that it was called by.
* Screenshots (camera icon) shows what the user sees.
* Deeper review:
  * Queuing:
    * Lower priority than critical resources (e.g., images vs js/css)
    * Unavailable TCP socks - about to free up
    * HTTP1 allows only 6 connections per origin
    * Time spent on disk cache entries (typically very fast)
  * Stalled/Blocking
    * Similiar to Queuing
    * Usually the proxy server for negotiation
  * Proxy negotiation
    * Timestamp of how long proxy negotiations took
  * DNS Lookup
    * Every new domain requires a full round trip to do the DNS lookup
  * Initial Connection / Connecting
    * Time it took to establish a connection, including TCP handshake/retries and negotiating a SSL.
  * Request Sent / Sending
    * Time spent issuing the network request. Typically a fraction of a millisecond.
  * Waiting (TTFB)
    * Time spent waiting on initial response, also known as Time To First Byte.  Captures roundtrip and spend time waiting.
  * Content Download / Downloading
    * Time spent receiving the response data.
* Try Network Throttling (3G) to get user experience

## Sources tab

* 'Workspace' sources tab.  Just drag a folder into 'Sources'.
  * Run app as localhost, then create workspace and map files to save.
* In the 'call stack', can right-click on any script that are 3rd party, and can 'Blackbox Script'.  That script will not be part of debugging, and won't step into it.

### Performance

* github.com/wilsonpage/fastdom
  * Window.requestAnimationFrame() - saves writing of DOM in batch.
* Check out paint flashing => Hit 'esc' for the console drawer and click on 'Rendering'
* And FPS meter
* While looking at pain flashing, just guess and deleting elements might help.
* CSS: 'will-change: transform' -> Will send to GPU.  Works best for 'fixed position' elements.

### Profiles

* Heap Snapshot
  * Take 2 snapshots then do a comparison.  This will be clear if there are any memory leaks.

### Resources

* @chromedevtools
* @firefoxdevtools
* @edgedevtools
* @paul_irish
* @addyosmani
* @jkup
* http://jankfree.org
* https://developers.google.com/web/tools/chrome-devtools/

## Vim Resources

* https://vimgifs.com/
* http://vim-adventures.com/

## Server Management

### Command-line tips

* cat - view file contents
* cat key.pub | pbcopy (puts it into clipboard)
* less - view file contents
* nslookup mnmm.co

### Key Management (OSX)

* Navigate to the ssh folder:
  * cd ~/.ssh
  * ssh-keygen -t rsa
* Do NOT leave the name default - change the name to something relevant to the project.
  * my_key (use underscore)
* Alway give the PUBLIC KEY, NOT the PRIVATE key.  Public keys end with .pub.

### Running a server on Digital Ocean, or elsewhere with similar features

Login:

* `ssh -i ~/.ssh/fem_digitalocean kenny@159.203.235.61` - logging in using the SSH key.
* Using your password: `ssh kenny@159.203.235.61`
* `less known_host file` (inside .ssh) shows the places and fingerprints.
* `top` runs activity/process monitor. `q` to quit.
* `apt-get install htop` - install prettier version of top.

#### Setup the server

* `apt-get update` - update software
* `apt-get upgrade` - install the updates
* `add $USERNAME` - create a new user. Never run as root. Default: `kenny`
* `usermod -aG sudo kenny` - add user `kenny` to sudo group
* `su kenny` - switch to user `kenny`
* `cat /var/log/auth.log` - test for access. Default access should be denied.
* If need access use `sudo`. Use `sudo!!` to replay the previous command in sudo.

#### Really setup the server

Use the SSH key rather than passwords.

* `cat fem_digitalocean.pub | ssh kenny@159.203.235.61 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"`
  * get the public key
  * ssh into the server
  * create the .ssh directory
  * get the key an put it into authorized keys
* Disable root login
  * `sudo vi /etc/ssh/sshd_config`
  * `PermitRootLogin no`
  * `sudo service sshd restart`

* Install nginx
  * sudo apt install nginx
  * sudo service nginx start
  * sudo less /etc/nginx/sites-available/default 

* sudo apt install git
* sudo apt install nodejs npm
* nodejs --version
* sudo ln -s /usr/bin/nodejs /usr/bin/node - symlink nodejs to node
* node --version
* sudo mkdir -p /var/www/ - make a web directory if doesn't exist
* sudo chown -R $USER:$USER /var/www
* git clone https://github.com/young/Dev-Ops-for-Frontend.git
* mv Dev-Ops-for-Frontend/ app/
* cd app/

Install NPM

* npm i - install npm packages from package.json
* node app.js - run the app

#### Configure Nginx

sudo vi /etc/nginx/sites-available/default
Modify this block
`location / {
    proxy_pass http://127.0.0.1:3001/;
}`
Restart nginx: `sudo service nginx restart`
Start express server: `node app.js`

Install gulp - `npm i -g gulp`
It will break - fix permissions
`sudo mkdir -p /usr/local/lib/node_modules`
https://docs.npmjs.com/getting-started/fixing-npm-permissions

#### Keep Processes Running - Run Forever

* pwd // /var/www/app
* `npm i -g forever`
* `forever start app.js` - start app.js
* `forever stopall` - start all
* Log forever - `sudo mkdir -p /var/log/forever`
* Change owner to current user - `sudo chown -R $USER /var/log/forever`
* Start logging - `forever start app.js >> /var/log/forever/forever.log`

#### Tail a log - See the end of file in realtime

* `sudo tail -f /var/log/auth.log`
* Above is cool to watch logging in requests.

#### Scripts to deploy

Inside package.json
"scripts": {
  "deploy": "gulp build && forever stopall & forever start app.js >> /var/log/forever/forever.log"
},

* Run the deploy gulp: `npm run deploy`

## VS Code

I installed font 'Operator Mono', but was missing something called 'ligatures', which makes symbols pretty like: == and === and =>, etc.

I had to do couple of things:
Create new font using 'Operator Mono' to include 'ligatures' with this: https://github.com/kiliman/operator-mono-lig

Then is VSCode, add this to my settings:
`Add "editor.fontLigatures": true`

It was a little involved, but took about 30 mins.

Right now, the font looks too bold, b/c the tool uses Medium weight. Trying to figure out if I can use the lighter weight.

I FIGURED IT OUT and forked the project. https://github.com/kennyklee/operator-mono-book-lig

## TypeScript Checking in VSCode

TS Config example:
Check JS & be explicit for all:
![screenshot](http://d.weblife.us/oDlHG+ "Screenshot")
