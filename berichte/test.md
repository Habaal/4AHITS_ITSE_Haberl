
# Übung (Online password guessing)

## 1) Ziel der Übung

Ziel dieser Übung ist es, einen **Online Password Guessing Attack** auf die **rlogin-** und **SSH-Ports** der Metasploitable-VM durchzuführen. Dafür wird das in Kali Linux vorinstallierte Tool **hydra** verwendet. Anhand vorgegebener Benutzer- und Passwortlisten sollen erfolgreiche Login-Kombinationen identifiziert werden.

---

## 2) Vorbereitung

Verwendete Dienste:

* **rlogin** (TCP 513)
* **SSH** (TCP 22)

Verwendete Listen:

* Benutzerliste: vorgegebene Liste (msfadmin, user, root, admin, …)
* Passwortliste: vorgegebene Liste (msfadmin, user, toor, password, …)

Zielsystem:

* Metasploitable VM (Laborumgebung)

---

## 3) Online Password Guessing auf SSH

### Befehl:

```sh
hydra -L users.txt -P passwords.txt ssh://192.168.5.51
```

### Ergebnis:

```sh
[22][ssh] host: 192.168.5.51   login: msfadmin   password: msfadmin
```

### Interpretation:

* Für den SSH-Dienst wurde eine **gültige Benutzer/Passwort-Kombination** gefunden.
* Der Benutzer **msfadmin** verwendet das sehr schwache Passwort **msfadmin**.
* Ein Login per SSH ist damit erfolgreich möglich.

---

## 4) Online Password Guessing auf rlogin

### Befehl:

```sh
hydra -L users.txt -P passwords.txt rlogin://192.168.5.51
```

### Ergebnis / Beobachtung:

* rlogin erlaubt auf Metasploitable einen **Root-Login ohne Passwort**.
* Ein klassisches Password Guessing ist hier nicht notwendig.
* Der Zugriff ist aufgrund einer Fehlkonfiguration sofort möglich.

---

## 5) Welche erfolgreichen User/Password-Kombinationen wurden gefunden?

Erfolgreiche Kombinationen:

* **SSH:**

  * Benutzer: `msfadmin`
  * Passwort: `msfadmin`

* **rlogin:**

  * Benutzer: `root`
  * Passwort: **keines erforderlich** (passwortloser Login)

---

## 6) Sicherheitsbewertung

Die Übung zeigt deutlich:

* Schwache oder identische Benutzer/Passwort-Kombinationen können leicht erraten werden.
* Online Password Guessing ist besonders effektiv, wenn:

  * keine Account-Sperren existieren,
  * schwache Passwörter verwendet werden,
  * veraltete Dienste wie rlogin aktiv sind.

In realen Systemen müssen daher starke Passwörter, sichere Dienste (SSH) und Schutzmechanismen wie Rate-Limiting eingesetzt werden.

---
