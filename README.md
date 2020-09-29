# Projet6-OC
Open Classroom project : AWS deployment

 Ce script a pour fonction le déploiement d’instances EC2 sur AWS avec une copie d’instantanés.
************************************************
	-Création d’une paire de clef Amazon AWS CLI:
Commençons avec les commandes :
#pip3 install awscli
#pip3 install --user --upgrade awscli

Maintenant allons créer l’utilisateur sur AWS:
https://console.aws.amazon.com/iam/home?#/users 
Le compte crée doit avoir au minimum des droits administrateur sur « EC2FullAccess ». La création du compte faite sélectionnez l’utilisateur crée et cliquez sur download .csv (Ou, depuis le site récupérez les valeurs « Access Key ID » & « Secret access key »)

Retournons sur le poste ansible, pour utiliser la commande 
#aws configure

AWS Access Key ID [***************]: 
AWS Secret Access Key [************]: 
Default region name [eu-west-3]: 
Default output format [json]:

Remplissez les champs, la région en pré-selection sur ce module est Paris (eu-west-3)

Si vous avez une instance déjà ouverte, vous pouvez faire un test de fonctionnement avec la commande:
#aws ec2 describe-instances

Si la configuration est faire, je vous invit à faire un « export » des valeurs:

#export AWS_ACCESS_KEY_ID=*******************
#export AWS_SECRET_ACCESS_KEY=****************
************************************************
Les variables du script :
	-Instance_type : T2.micro (Changement du type d’instance possible, avec leurs performances propre).
	-Security_group : Renseigne le compte ou lots de comptes ayant droit que vous allez configurer sur les instances
	-Image : Instantané de l’instance à déployer sur les nouvelles machines ( exemple des Virtual Machine : Windows serveur 2016, Ubuntu apache …) la configuration de l’instantané est à réaliser manuellement.
	-Region : eu-west-3  Vous pouvez choisir dans quelle région vos instances seront hébergés par AWS, ici nos machines sont hébergés sur Paris.
	-Count : Défini le nombre d’instances crées avec les configuration ci dessus. 

************************************************
Modifiez le fichier hosts pour renseigner la ligne suivante :
[local]
localhost ansible_connection=local ansible_python_interpreter=/usr/bin/python3

Quand vous allez lancer le script les nouvelles IP des machines crées seront enregistrés dans un fichier log :
Vous pouvez retrouver le chemin du fichier à utiliser dans le script à ce niveau :

name: Add the newly created EC2 instance(s) to the local host group
        local_action: lineinfile 
                      path=/etc/ansible/logs/****.txt
Les adresses seront remontées automatiquement dans le fichier.
( il est nécéssaire de faire un « chmod » sur le fichier log pour permettre à ansible la modification de celui-ci)

lacement du module :
#ansible-playbook -i /etc/ansible/hosts /etc/ansible/roles/AWS/Projet6,yml 
