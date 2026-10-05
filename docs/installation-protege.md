Complete guide for downloading, installing, and configuring Protégé on Ubuntu with 2x HiDPI display scaling:

1. **Download and Extract Protégé:** Obtain the Linux release package.
Download the Protégé archive from the Stanford release server or GitHub, then extract the `.tar.gz` package:

```bash
wget https://github.com/protegeproject/protege-distribution/releases/download/protege-5.6.9/Protege-5.6.9-linux.tar.gz
tar -xzf Protege-5.6.9-linux.tar.gz
cd Protege-5.6.9-linux

```


2. **Move Protégé to System Directory:**
Move the extracted Protégé directory to `/opt/` for system-wide access:

```bash
sudo cp -r Protege-5.6.9 /opt/protege

```


3. **Create Scaled Terminal Shortcut:**
Create an executable wrapper script in `/usr/local/bin/` so running `protege` from any directory opens it with `GDK_SCALE=2`:

```bash
sudo bash -c 'cat << "EOF" > /usr/local/bin/protege
#!/bin/bash
GDK_SCALE=2 exec /opt/protege/protege "$@"
EOF'

sudo chmod +x /usr/local/bin/protege

```


4. **Install Desktop Application Shortcut:**
Copy the desktop shortcut to `/usr/share/applications/` and configure the executable path, scale variable, and application icon:

```bash
sudo cp /opt/protege/edu.stanford.protege.desktop /usr/share/applications/

sudo sed -i 's|Exec=.*|Exec=env GDK_SCALE=2 /opt/protege/protege|' /usr/share/applications/edu.stanford.protege.desktop
sudo sed -i 's|Icon=.*|Icon=/opt/protege/edu.stanford.protege-128.png|' /usr/share/applications/edu.stanford.protege.desktop

```


5. **Launch and Verify:**
You can now open Protégé in two ways:

* **Terminal:** Type `protege` from any directory.
* **GUI:** Press `Super` (Windows key) and search for **Protégé**.


---

> **Tip:** To uninstall Protégé in the future, remove `/opt/protege`, `/usr/local/bin/protege`, and `/usr/share/applications/edu.stanford.protege.desktop`.
