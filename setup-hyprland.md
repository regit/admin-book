custom google chrome

~/.local/share/applications/


Exec=/usr/bin/google-chrome-stable --password-store=gnome-libsecret %U


Doc

https://wiki.archlinux.org/title/GNOME/Keyring

PAM

Add gnome keyring at start

nvim UserConfigs/Startup_Apps.conf

Add
exec-once = gnome-keyring-daemon --start --components=secrets

Wayland in chrome

 --enable-features=UseOzonePlatform --ozone-platform=wayland

https://www.reddit.com/r/Fedora/comments/rkzp78/make_chrome_run_on_wayland_permanently/?rdt=55305


SSH agent

https://www.lorenzobettini.it/2023/09/hyprland-and-ssh-agent/


systemctl enable --user gcr-ssh-agent.service
systemctl start --user gcr-ssh-agent.service

 See logs
journalctl --user -u gcr-ssh-agent.service

