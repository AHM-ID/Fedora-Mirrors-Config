# Configuring Fedora 41 with European Mirrors

This guide provides a detailed, step-by-step process to configure Fedora 41 to use five reliable European mirrors for efficient package management. Each step is designed for clarity and ease of use, ensuring optimal performance.

---

## Mirrors Used
- `https://mirror.dogado.de/fedora/linux/` (Germany, Dogado GmbH)  
- `https://mirror.leaseweb.com/fedora/linux/` (Netherlands, Leaseweb)  
- `https://ftp.belnet.be/mirror/fedora/linux/` (Belgium, BELNET)  
- `https://www.mirrorservice.org/sites/dl.fedoraproject.org/pub/fedora/linux/` (UK, Janet Network)  
- `https://mirror.in2p3.fr/pub/fedora/linux/` (France, IN2P3)  

All mirrors are verified accessible as of April 09, 2025, and support Fedora 41 repositories.

---

## Step 1: Test Mirror Performance
Evaluate the speed and latency of each mirror to determine the optimal order.

1. **Test Download Speed**:  
   ```bash
   wget -O /dev/null https://mirror.dogado.de/fedora/linux/updates/41/Everything/x86_64/repodata/repomd.xml
   ```
   Repeat for:  
   - `https://mirror.leaseweb.com/fedora/linux/`  
   - `https://ftp.belnet.be/mirror/fedora/linux/`  
   - `https://www.mirrorservice.org/sites/dl.fedoraproject.org/pub/fedora/linux/`  
   - `https://mirror.in2p3.fr/pub/fedora/linux/`  
   Record the download speeds (e.g., MB/s).

2. **Test Latency**:  
   ```bash
   ping -c 10 mirror.dogado.de
   ```
   Repeat for:  
   - `mirror.leaseweb.com`  
   - `ftp.belnet.be`  
   - `www.mirrorservice.org`  
   - `mirror.in2p3.fr`  
   Note the average ping times.

3. **Order Mirrors**: Arrange the `baseurl` entries in Step 3 based on fastest download speed or lowest latency.

---

## Step 2: Backup Current Configuration
Create a backup of your existing repository configuration for safety.

1. **Backup Command**:  
   ```bash
   sudo cp -r /etc/yum.repos.d /etc/yum.repos.d.bak
   ```

2. **Verify Backup**:  
   ```bash
   ls /etc/yum.repos.d.bak
   ```
   Ensure files like `fedora.repo` and `fedora-updates.repo` are present.

---

## Step 3: Edit Repository Files
Configure Fedora 41 to use the specified mirrors by updating the repository files.

### Edit `fedora.repo`
1. Open the file:  
   ```bash
   sudo nano /etc/yum.repos.d/fedora.repo
   ```

2. Replace with the following content:  
   ```ini
   [fedora]
   name=Fedora $releasever - $basearch
   baseurl=https://mirror.dogado.de/fedora/linux/releases/41/Everything/$basearch/os/
   baseurl=https://mirror.leaseweb.com/fedora/linux/releases/41/Everything/$basearch/os/
   baseurl=https://ftp.belnet.be/mirror/fedora/linux/releases/41/Everything/$basearch/os/
   baseurl=https://www.mirrorservice.org/sites/dl.fedoraproject.org/pub/fedora/linux/releases/41/Everything/$basearch/os/
   baseurl=https://mirror.in2p3.fr/pub/fedora/linux/releases/41/Everything/$basearch/os/
   enabled=1
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch

   [fedora-debuginfo]
   name=Fedora $releasever - $basearch - Debug
   baseurl=https://mirror.dogado.de/fedora/linux/releases/41/Everything/$basearch/debug/
   baseurl=https://mirror.leaseweb.com/fedora/linux/releases/41/Everything/$basearch/debug/
   baseurl=https://ftp.belnet.be/mirror/fedora/linux/releases/41/Everything/$basearch/debug/
   baseurl=https://www.mirrorservice.org/sites/dl.fedoraproject.org/pub/fedora/linux/releases/41/Everything/$basearch/debug/
   baseurl=https://mirror.in2p3.fr/pub/fedora/linux/releases/41/Everything/$basearch/debug/
   enabled=0
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch

   [fedora-source]
   name=Fedora $releasever - Source
   baseurl=https://mirror.dogado.de/fedora/linux/releases/41/Everything/source/SRPMS/
   baseurl=https://mirror.leaseweb.com/fedora/linux/releases/41/Everything/source/SRPMS/
   baseurl=https://ftp.belnet.be/mirror/fedora/linux/releases/41/Everything/source/SRPMS/
   baseurl=https://www.mirrorservice.org/sites/dl.fedoraproject.org/pub/fedora/linux/releases/41/Everything/source/SRPMS/
   baseurl=https://mirror.in2p3.fr/pub/fedora/linux/releases/41/Everything/source/SRPMS/
   enabled=0
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
   ```

3. Save and exit: Press `Ctrl+O`, `Enter`, then `Ctrl+X`.

---

### Edit `fedora-updates.repo`
1. Open the file:  
   ```bash
   sudo nano /etc/yum.repos.d/fedora-updates.repo
   ```

2. Replace with the following content:  
   ```ini
   [updates]
   name=Fedora $releasever - $basearch - Updates
   baseurl=https://mirror.dogado.de/fedora/linux/updates/41/Everything/$basearch/
   baseurl=https://mirror.leaseweb.com/fedora/linux/updates/41/Everything/$basearch/
   baseurl=https://ftp.belnet.be/mirror/fedora/linux/updates/41/Everything/$basearch/
   baseurl=https://www.mirrorservice.org/sites/dl.fedoraproject.org/pub/fedora/linux/updates/41/Everything/$basearch/
   baseurl=https://mirror.in2p3.fr/pub/fedora/linux/updates/41/Everything/$basearch/
   enabled=1
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch

   [updates-debuginfo]
   name=Fedora $releasever - $basearch - Updates Debug
   baseurl=https://mirror.dogado.de/fedora/linux/updates/41/Everything/$basearch/debug/
   baseurl=https://mirror.leaseweb.com/fedora/linux/updates/41/Everything/$basearch/debug/
   baseurl=https://ftp.belnet.be/mirror/fedora/linux/updates/41/Everything/$basearch/debug/
   baseurl=https://www.mirrorservice.org/sites/dl.fedoraproject.org/pub/fedora/linux/updates/41/Everything/$basearch/debug/
   baseurl=https://mirror.in2p3.fr/pub/fedora/linux/updates/41/Everything/$basearch/debug/
   enabled=0
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch

   [updates-source]
   name=Fedora $releasever - Updates Source
   baseurl=https://mirror.dogado.de/fedora/linux/updates/41/Everything/source/SRPMS/
   baseurl=https://mirror.leaseweb.com/fedora/linux/updates/41/Everything/source/SRPMS/
   baseurl=https://ftp.belnet.be/mirror/fedora/linux/updates/41/Everything/source/SRPMS/
   baseurl=https://www.mirrorservice.org/sites/dl.fedoraproject.org/pub/fedora/linux/updates/41/Everything/source/SRPMS/
   baseurl=https://mirror.in2p3.fr/pub/fedora/linux/updates/41/Everything/source/SRPMS/
   enabled=0
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-$releasever-$basearch
   ```

3. Save and exit: Press `Ctrl+O`, `Enter`, then `Ctrl+X`.

---

## Step 4: Optimize DNF Configuration
Enhance DNF performance for better download speeds and reliability.

1. Open the configuration file:  
   ```bash
   sudo nano /etc/dnf/dnf.conf
   ```

2. Replace or update with:  
   ```ini
   [main]
   gpgcheck=1
   installonly_limit=3
   clean_requirements_on_remove=True
   max_parallel_downloads=10
   minrate=100k
   timeout=15
   fastestmirror=False
   deltarpm=True
   ```

3. Save and exit: Press `Ctrl+O`, `Enter`, then `Ctrl+X`.

---

## Step 5: Test the Configuration
Validate the new mirror setup.

1. **Clear DNF Cache**:  
   ```bash
   sudo dnf clean all
   ```

2. **Rebuild Metadata**:  
   ```bash
   sudo dnf makecache
   ```

3. **Check for Updates**:  
   ```bash
   sudo dnf check-update
   ```

---

## Step 6: Monitor and Maintain
Ensure ongoing performance and maintain a rollback option.

1. **Monitor Download Speeds**:  
   ```bash
   sudo dnf update -v
   ```
   Use verbose mode to view speeds from each mirror.

2. **Restore Backup (if needed)**:  
   ```bash
   sudo cp -r /etc/yum.repos.d.bak/* /etc/yum.repos.d/
   ```

3. **Explore Additional Mirrors (optional)**: Visit [Fedora MirrorManager](https://mirrormanager.fedoraproject.org/) for alternatives.

---

## Notes
- Debuginfo and source repositories are disabled by default (`enabled=0`). Enable them if required:  
  ```bash
  sudo dnf config-manager --set-enabled fedora-debuginfo updates-debuginfo
  ```
- Adjust the mirror order in Step 3 based on Step 1 results for best performance.
