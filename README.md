cat << EOF > README.md
# 🔐 Chiffrement AES en ligne de commande sous Linux (avec OpenSSL)

Ce guide explique comment chiffrer et déchiffrer des fichiers en utilisant AES via `openssl` sur Linux. Pas besoin de programmation, tout se fait en terminal.

---

## ✅ Prérequis

OpenSSL doit être installé (c’est le cas par défaut sur la plupart des distributions Linux).

Vérification :
```bash
openssl version
```

Si ce n’est pas installé :
```bash
sudo apt update
sudo apt install openssl
```

---

## 🔒 1. Chiffrer un fichier avec AES

```bash
openssl enc -aes-256-cbc -salt -in fichier.txt -out fichier.txt.enc
```

- `-aes-256-cbc` : AES avec une clé 256 bits en mode CBC
- `-salt` : ajoute du sel pour renforcer le chiffrement
- `-in` : fichier en clair
- `-out` : fichier chiffré
- 👉 Il te demandera un mot de passe

---

## 🔓 2. Déchiffrer un fichier AES

```bash
openssl enc -d -aes-256-cbc -in fichier.txt.enc -out fichier_dechiffre.txt
```

- `-d` : mode déchiffrement

---

## 🔑 3. Chiffrer avec une clé et un IV personnalisés

### Générer une clé (256 bits = 32 octets) et un IV (16 octets) :
```bash
openssl rand -hex 32 > key.hex
openssl rand -hex 16 > iv.hex
```

### Lire les valeurs :
```bash
KEY=$(cat key.hex)
IV=$(cat iv.hex)
```

### Chiffrer avec clé et IV :
```bash
openssl enc -aes-256-cbc -K $KEY -iv $IV -in fichier.txt -out fichier.enc
```

### Déchiffrer :
```bash
openssl enc -d -aes-256-cbc -K $KEY -iv $IV -in fichier.enc -out dechiffre.txt
```

---

## 🧪 4. Vérifier le contenu chiffré

Afficher un aperçu du fichier chiffré en hexadécimal :
```bash
hexdump -C fichier.enc | head
```

---

## 📄 5. Exemple complet

```bash
# Créer un fichier de test
echo "ceci est un message secret" > message.txt

# Chiffrer
openssl enc -aes-256-cbc -salt -in message.txt -out message.txt.enc

# Déchiffrer
openssl enc -d -aes-256-cbc -in message.txt.enc -out message_dechiffre.txt

# Vérifier
cat message_dechiffre.txt
```

---

## 🧼 Bonnes pratiques

- Ne stocke pas la clé et l’IV en clair si tu les utilises manuellement
- Privilégie les outils comme GPG pour la gestion de clés asymétriques
- AES avec OpenSSL est utile pour des cas simples, mais attention aux faiblesses si mal utilisé

EOF
