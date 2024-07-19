# vncjew-archlinux
Install vncjew on Arch Linux and Arch-based distros. For Debian-based distros, go [here](https://github.com/wawaloll/installvncjew/).

# Install dependencies
## yay way
I recommend you to install it via AUR helper, like yay. `yay -S go wget git python python-numpy nmap masscan iptables libjpeg libvncserver websockify screen` <br>
If you don't have an AUR helper, there's a guide how to get it: <br>
Run `pacman -S git` as root. Then, do `git clone https://aur.archlinux.org/yay.git`, go to **yay** directory and run `makepkg -si`. DON'T RUN IT AS ROOT!!! <br>
Then, run `yay -S go wget git python python-numpy nmap masscan iptables libjpeg libvncserver websockify`. <br>

We're done with deps.
## Non-yay way
Run `pacman -Sy go wget git python python-numpy nmap masscan iptables libjpeg libvncserver screen` as root. <br>
Now, we have to install websockify from AUR (Arch Linux User Repository) <br> 
Run `git clone https://aur.archlinux.org/websockify.git`, go to **websockify** directory and run `makepkg -si`. DON'T RUN IT AS ROOT!!! <br>

We're done with deps.

# Editing stuff
You need a braincell. Shocking, right? <br>
Clone vncjew repository: `git clone https://github.com/jsteel2/vncjew` <br>
Then, go to **vncjew/server** directory. <br>
I dunno how to compile it, sooo <br>
Run `wget https://github.com/zanikitas/vncjew-archlinux/raw/main/vncscreenshot && chmod +x vncscreenshot` <br>
Now, use your favorite text editor like `vim`, and edit `config.go`: <br>
Replace `"admin": "pass"` with your desired admin password, you will need it to start/stop scans, refresh screenshots and delete hosts. <br>
Replace `"client": "pass"` with your desired client password, you will need it for clients. <br>
Replace `var CFGIPInfoToken = "token"` with your IPInfo token, register and get it [here](https://ipinfo.io/). <br>
(optional, but very recommended) Replace `var CFGPasswords` with this: <br>
`var CFGPasswords = []string{"123456", "password", "admin", "user", "default", "", "123456789", "111111", "password", "qwerty", "abc123", "12345678", "password1", "1234567", "123123", "pu", "god", "sex", "secret"}`<br>
You still can keep the defaults, but add `, ""` after all passwords! It will add VNCs with no password. Also, you can add your own passwords. <br>
Now, go into the "client" folder and edit "main.go" file: <br>
```
var server = "***REMOVED***"
var password = "***REMOVED***"
```
Replace the server variable with your server IP (and add :8080, example: 69.69.69.69:8080). If you're going to run server and client on the same instance, replace it `localhost:8080` <br>

# Installing noVNC
Go into the **vncjew/server** directory, then run `git clone https://github.com/novnc/noVNC novnc`.

# Running vncjew

## Building
Run `go build` in server directory, for client not needed.

## Running on the same machine
Use screen.<br>
`screen ./vncjew` in the server directory.<br>
`screen go run main.go` in the client directory.<br>
To detach, do ctrl+a+d<br>

## Not on the same machine
Run `./vncjew` in the host machine in the server directory.<br>
Run `go run main go` in the client machine in the client directory.<br>

# Run/Stop scans
Go to `[ur ip]:8080/admin/start` to start the scan. <br>
Go to `[ur ip]:8080/admin/stop` to stop the scan. <br>
It will ask you to auth, use **admin** user and admin password.

# Troubleshooting
If you get this error in the client: `websocket.Dial wss://[ur ip]:8080/api/client: tls: first record does not look like a TLS handshake`, then you're running http. Try changing these lines: <br>
`sec := "s"` to `sec := ""`<br>
and `Origin: &url.URL{Scheme: "https", Host: server}` to `Origin: &url.URL{Scheme: "http", Host: server}`<br>
