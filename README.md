# Description des étapes du TP2 sur ACTIVE DIRECTORY

### Auteur : Yawo Elfried NOUMEDOR

## Pré-requis pour le lab ACTIVE DIRECTORY

- Windows Server Standard 2019 nouvellement installé
- Une adresse IP **statique** (192.168.245.145/24) configurée sur le serveur.
- Exécution du script en tant qu’**Administrateur**
- Un fichier CSV des utilisateurs (utilisateurs.csv)
- Les scripts ainsi que les fichier .bat (pour les excécutions automatiques)

## Fichiers fournis

- `AD-laplateforme_io-forest.ps1` : Script PowerShell pour créer le domaine Active Directory.
- `bat_AD-laplateforme_io-forest.bat` : Script Batch pour exécuter automatiquement le script PowerShell de creation du domaine AD.
- `AD-user_groups-Creation.ps1` : Script PowerShell pour créer les utilisateurs et les ajouter aux groupes qui sont crées s'ils n'existent pas.
- `bat_AD-user_groups-Creation.bat` : Script Batch pour exécuter automatiquement le script PowerShell de création des utilisateurs.


## Étapes détaillées

### 1. Installation du rôle AD DS
# === Définition des paramètres ===

$NomDomaine = "laplateforme.io"      # Nom de domaine complet (FQDN)
$NomNetBIOS = "LAPLATEFORME"            # Nom NetBIOS
$MotDePasse = "P@ssw0rd" | ConvertTo-SecureString -AsPlainText -Force  # Mot de passe du DSRM

# === Étape 1 : Installation des services AD-DS ===
```powershell
Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

# === Étape 2 : Promouvoir ce serveur en tant que contrôleur de domaine principal (DC root) ===
```powershell
Install-ADDSForest `
    -DomainName $NomDomaine `
    -DomainNetbiosName $NomNetBIOS `
    -SafeModeAdministratorPassword $MotDePasse `
    -InstallDNS `
    -Force:$true
```
### 2. Création d’une UO (Unité d’Organisation)

```powershell
New-ADOrganizationalUnit -Name "Utilisateurs" -Path "DC=laplateforme,DC=io"
```


### 3. Structure du fichier CSV (utilisateurs.csv)

    Nom,prenom,groupe1,groupe2,groupe3,groupe4,groupe5,groupe6

    ```
    ALEXANDRE,MARCELLINE,Animation,,,,,
    ARAGON,ISABELLE,Animation,,,,,
    AVARO,MARINA,As,Médical,,,,
    BERNARD,ISABELLE,As,Médical,,,,
    BOUFFIER,STEPHANE,ASH,Hébergement,,,,
    BOUZIANE,FATIHA,cadredesanté,Cadres,Médical,,,
    CARLIER,CHANTAL,Comptable,Administratif,,,,
    THILLOT,MARC,Directeur,Cadres,Hébergement,Technique,Administratif,Animation
    GALLIEN,CAROLE,MaîtressedeMaison,Cadres,Hébergement,,,
    GRIVEAUX,PATRICIA,Médecin,Cadres,Médical,,,
    LARGUIER,SILVANIA,Psychologue,Cadres,Médical,,,
    MALAURE,OPHYLANADRA,ASH,Hébergement,,,,
    PRATABUY,MYRIAM,Secrétaire,Administratif,,,,
    SAHTIT,OIFAA,IDE,Médical,,,,
    SALVADOR,GLADYS,IDE,Médical,,,,
    SCHNEIDER,EMILE,Technique,Cadres,,,,
    VIGNOLO,VERONIQUE,Animation,Cadres,Hébergement,Administratif,,
    ```

### 4. Exécution du script

  - Lancement du script en tant qu’Administrateur
    ```powershell
    .\Peuplement-AD.ps1
    ```

### 5. Journalisation
```
Le script crée un fichier `log_peuplement.txt` contenant toutes les opérations effectuées à la racine du lecteur C:.
```

## Résultat
```
- Les utilisateurs sont créés dans l’UO définie.
- Ils reçoivent le mot de passe `Azerty_2025!` et doivent le changer à la première connexion.
- Ils sont ajoutés aux groupes spécifiés, créés si nécessaire.```
