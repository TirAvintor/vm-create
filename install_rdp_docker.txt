#!/bin/bash

echo "=========================================="
echo "     RDP BY GAMERBOY_l                    "
echo "          Debian 11 / Google IDX          "
echo "=========================================="
sleep 1

echo "[1/8] Updating system..."
sudo apt update && sudo apt upgrade -y

echo "[2/8] Installing XFCE Desktop, XRDP, DBUS, Firefox..."
sudo apt install xfce4 xfce4-goodies xrdp dbus-x11 firefox-esr -y

echo "[3/8] Configuring .xsession for XRDP..."
echo "export \$(dbus-launch)" > ~/.xsession
echo "xfce4-session" >> ~/.xsession

echo "[4/8] Fixing .xsession permissions..."
sudo chown $(whoami):$(whoami) ~/.xsession

echo "[5/8] Adding XRDP user to ssl-cert group..."
sudo adduser xrdp ssl-cert

echo "[6/8] Enabling and restarting XRDP service..."
sudo systemctl enable xrdp
sudo systemctl restart xrdp

echo "[7/8] Fixing Firefox cannot launch under XRDP..."
sudo sed -i 's/^Exec=firefox-esr.*/Exec=firefox-esr --no-sandbox --disable-seccomp/' /usr/share/applications/firefox-esr.desktop

echo "[8/8] Installation Complete!"
echo "=========================================="
echo " ✅ XRDP is ready"
echo " ✅ XFCE desktop configured"
echo " ✅ DBus fix applied"
echo " ✅ Firefox fixed for XRDP"
echo "=========================================="
echo "Now you can connect using Windows RDP."
