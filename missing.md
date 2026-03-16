
1) Initiales OS-Flashing / First Boot
   Nicht Teil des Projekts:
   Raspberry Pi Imager benutzen
   Hostname beim Flashen setzen
   SSH aktivieren
   SSH-Keys provisionieren
   WLAN-Zugangsdaten setzen
   Das ist jetzt bewusst vorgelagert; siehe README.md:23-38.
2) SSH-Key-Generierung
   In old.md war bei „Controller Mode“ von „Generating SSH and AGE keys“ die Rede.
   Im aktuellen Projekt sehe ich dazu keine Generierung. Stattdessen wird vorausgesetzt:
   lokaler SSH-Key existiert (README.md:27-31)
   Public Key liegt schon auf den Nodes
   Ansible kann sich einloggen
   Also: SSH-Key-Nutzung ja, Generierung nein.
3) AGE-Key-Generierung
   Das Projekt kann einen vorhandenen Age-Key als Kubernetes Secret einspielen (roles/k3s/control-plane/addons/sops/tasks/main.yaml), aber es erzeugt ihn nicht.
   Also: SOPS/Age-Integration ja, Key-Erzeugung nein.
4) „Disable unused services“ nur teilweise
   Aus old.md fehlen im Repo die alten manuellen Tweaks für:
   disable-wifi
   wpa_supplicant
   hciuart
   triggerhappy
   triggerhappy.socket
   dphys-swapfile / Swap deaktivieren
   Ich habe diese Begriffe im Repo gesucht; außerhalb von old.md tauchen sie praktisch nicht als Tasks auf.
   Aktuell deaktiviert das Projekt nur:
   dtoverlay=disable-bt
   ACT LED
   Audio
   ModemManager
   Das ist also eine reduzierte, vorsichtigere Variante.
5) Verifikation nach dem Deployment
   Die alten Prüfkommandos sind nicht als Ansible-Checks automatisiert:
   watch flux get all
   kubectl get nodes
   kubectl get pods --all-namespaces -w
   Die Voraussetzungen dafür werden aber geschaffen:
   kubeconfig wird nach /home/{{ ansible_user }}/.kube/config kopiert
   KUBECONFIG wird in .profile gesetzt
   Flux-CLI wird optional installiert
