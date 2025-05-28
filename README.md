# Commandes de base d'OpenStack V1

> [!IMPORTANT]
> Sourcer le fichier OpenStack RC avant de commencer.

## BASE
#### Création paire de clés
> `openstack keypair create NOM_KEYPAIR > NOM_KEYPAIR`
<br>
> `chmod 600 ~/.ssh/NOM_KEYPAIR`

#### Création instance
> `openstack server create --image "NOM IMAGE" --flavor NOM_FLAVOR --key-name NOM_KEYPAIR --network NOM_RESEAU (de base : ext-net1) VM_NOM`

#### Verifier instances
> `openstack server list`

#### Connexion instance
> `ssh -i NOM_KEYPAIR user@ip`

#### Reboot à froid
> `openstack server reboot --hard NOM_VM`

#### Redimensionnement de l’instance
> `openstack server resize --flavor NOM_FLAVOR NOM_VM`

#### Création d’un snapshot
> `openstack server image create --name NOM_SNAPSHOT VM_NOM`

## Swift
OpenStack Swift est un service de stockage d'objets d'OpenStack qui permet de gérer des données dans le cloud de manière efficace et sécurisée. 
#### Création de conteneurs
> `openstack container create NOM_CONTAINER`

#### Vérifier la création
> `openstack container list`
<br>
> `openstack container show NOM_CONTAINER`

#### Configuration ACL (Access Control List) en accèss public lecture seul
> `openstack container set --property read-acl=".r:*,.rlistings" NOM_CONTAINER`

#### Transfert de fichiers
##### Téléversement
> `openstack object create NOM_CONTAINER FICHIER`
##### Vérification
> `openstack object list NOM_CONTAINER`

## Neutron
OpenStack Neutron est le composant de mise en réseau d'OpenStack qui fournit des fonctionnalités de réseau en tant que service. Il permet de gérer et de configurer des réseaux, des sous-réseaux, des routeurs et d'autres éléments de réseau de manière flexible et évolutive dans un environnement cloud.

#### Création d'un réseau Neutron
> `openstack network create NOM_NETWORK`

#### Lister les réseaux Neutron 
> `openstack network list`

#### Création d'un sous-réseau
> `openstack subnet create --network NOM_NETWORK --subnet-range 192.168.1.0/24 NOM_NETWORK_SUBNET`

#### Lister les sous réseaux
> `openstack subnet list`

#### Création d'un routeur
> `openstack router create NOM_ROUTEUR`

#### Verifier details routeur
> `openstack router show NOM_ROUTEUR`

#### Lister les réseaux externe
> `openstack network list --external`

#### Connecter le routeur à un réseau externe
> `openstack router set --external-gateway NOM_RESEAU_EXTERNE NOM_ROUTEUR`

#### Connecte un sous réseau à un routeur Neutron
> `openstack router add subnet NOM_ROUTEUR NOM_NETWORK_SUBNET`

## Cinder
Cinder est le service de stockage de blocs dans l'écosystème OpenStack. Il permet de fournir des volumes de stockage persistants pouvant être attachés à des instances Nova, offrant ainsi une solution de stockage similaire aux disques durs physiques dans un environnement cloud.

#### Créer un volume Cinder
> `openstack volume create --size TAILLE_EN_GB NOM_VOLUME`

#### Lister les volumes cinder
> `openstack volume list`

#### Attacher le volume à une instance
> `openstack server add volume NOM_VM NOM_VOLUME`

#### Dans la VM
> ##### Afficher les disques et partitions d'un system debian/ubuntu
> > `lsblk`
> 
> ##### Formatter le volume
> > `sudo mkfs.ext4 /dev/NOM_DISQUE (Exemple: /dev/sdb)`
> 
> ##### Monter le volume
> > `sudo mount /dev/NOM_DISQUE /mnt`

#### Détacher un volume d'une VM
> `openstack server remove volume NOM_VM NOM_VOLUME`

#### Supprimer le volume 
> `openstack volume delete NOM_VOLUME`
